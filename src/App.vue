<template>
    <div id="app">
        <h3 class="text-center p-3">{{ title }}</h3>
        <hr class="col-10">
        <div class="container">
            <div class="row">
                <div class="col-md-6 pt-3">
                    <div class="form-group">
                        <label for="programmer">Employee</label>
                        <select class="form-control" id="programmer" v-model="programmer">
                            <option v-for="employee in workers">{{ employee }}</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="projectName">Project</label>
                        <select class="form-control" id="projectName" v-model="projectName">
                            <option v-for="project in projects">{{ project }}</option>
                        </select>
                    </div>
                    <hr class="col-4">
                    <div class="input-group mb-3">
                        <input class="form-control" type="text" placeholder="add task" name="newTask" v-model="newTask" @keyup.enter="addTask(newTask)"/>
                        <div class="input-group-append">
                            <button class="btn btn-outline-primary" @click="addTask(newTask)">Add</button>
                        </div>
                    </div>
                    <div class="input-group mb-3">
                        <input class="form-control" type="text" placeholder="todo" name="todo" v-model="todo" @keyup.enter="addTodo(todo)"/>
                        <div class="input-group-append">
                            <button class="btn btn-outline-primary" @click="addTodo(todo)">Add</button>
                        </div>
                    </div>
                    <div class="input-group mb-3">
                        <textarea class="form-control" type="text" placeholder="Add comment" name="comment" v-model="comment"/></textarea>
                    </div>
                </div>
                <hr>
                <div class="col-md-6" ref="raport">
                    <div>
                        <div class="ml-3 mt-3">
                            <div class="row mt-1">
                                <b>Date:&nbsp</b> {{ today }}
                            </div>
                            <div class="row mt-1">
                                <b>Employee:&nbsp</b> {{ programmer }}
                            </div>
                            <div class="row mt-1">
                                <b>Project:&nbsp</b> {{ projectName }}
                            </div>
                            <div class="row">
                                <div class="mt-3">
                                    <b>DONE today:</b>
                                    <ul v-for="task in doneTask">
                                        <li>- {{ task }}</li>
                                    </ul>
                                </div>
                            </div>
                            <div class="row">
                                <div class="mt-3">
                                    <b>TODO tomorrow:</b>
                                    <ul v-for="todo in todoTask">
                                        <li>- {{ todo }}</li>
                                    </ul>
                                </div>
                            </div>
                            <div v-show="comment" class="row">
                                <b>Comment:&nbsp</b>
                                {{ comment }}
                            </div>
                        </div>
                    </div>
                </div>
          </div>
          <div class="mt-3 mb-3">
              <button class="btn btn-outline-primary" @click="exportToPDF">Generate report</button>
          </div>
        </div>
    </div>
</template>
<script>
import html2pdf from 'html2pdf.js';
var dayjs = require('dayjs')
dayjs().format()
export default {
    components: {
        html2pdf
    },
    data () {
        return {
            title:       'Quarantine Tasks Reporter',
            programmer:  null,
            projectName: null,
            today:       null,
            newTask:     null,
            todo:        null,
            comment:     null,
            doneTask:    [],
            todoTask:    [],
            unDoneTask:  [],
            fileName:    null,

            //Set your name/workers names and projects
            workers:  ['John Doe', 'Ian Smith', 'Leonard Sun'],
            projects: ['Shop','Api','Tests'],
        }
    },
    mounted(){
        this.setDate();
    },
    methods:{
        addTask: function(task){
            this.doneTask.push(task);
            this.newTask = '';
        },
        addTodo: function(todo){
            this.todoTask.push(todo);
            this.todo = '';
        },
        setProject: function(project){
            this.projectName = project;
        },
        setDate: function(){
            return this.today = dayjs().format('MM/DD/YYYY');
        },
        exportToPDF () {
            html2pdf(this.$refs.raport, {
                margin: 0,
                filename: this.setFileName,
                html2canvas: { scale: 6 },
                jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
            })
        }
    },
    computed: {
        setFileName: function(){
            return this.programmer + '_' + this.today + '.pdf';
        }
    },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
