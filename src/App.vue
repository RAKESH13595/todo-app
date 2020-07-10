<template>
  <div id="app">
    <nav class="navbar navbar-expand-md navbar-dark bg-primary">
        <span class="navbar-brand abs">To-Do App</span>
    </nav>
    <br>
    <cmp-new-task @create="addTask($event)"></cmp-new-task>
    
     <div class="container">
        <div class="row">
            <div class="col-sm">
              <hr>
              <div v-if="tasks.length>0">
                <button class="btn btn-danger" @click="deleteAll">Delete all Tasks</button>
                <span> Progress:  </span>
                <select v-model="selectedOption" @change="onChange($event)">
                  <option v-for="option in filterOptions" :key="option"
                  >{{option}}</option>
                </select>
                <hr>
                <div v-if="filteredTasks.length>0" class="row">
                <div class="col-sm-4" v-for="(task,index) in filteredTasks" :key="task.title + index">
                  <cmp-task  :style="{backgroundColor:'lightblue'}" class="card" style="width: 18rem;"  
                  :task="task"
                  :index="index"
                  @deleteTask="deleteTask($event)"
                  @editConfirm="editTask($event)"
                  ></cmp-task>
                  
                </div>
                </div>
                <h3 v-else>No task in "{{selectedOption}}" state</h3>
              </div>
              <h3 v-else>No tasks to be displayed. Please add tasks</h3>
        </div>
      </div>   
    </div>
    <br>

  </div>
</template>

<script>
import NewTask from './components/NewTask'
import Tasks from './components/Tasks'

export default {
  name: 'App',
  data:function(){
    return {
      tasks:[{title:'taskA',desc:"taskA desc",status:"Pending",id:0},{title:'taskB',desc:'taskB desc',id:1,status:"Completed"},
      {title:'taskC',desc:'taskC desc',status:"In Progress",id:2}],
      filterOptions:['All','Pending','In Progress','Completed'],
      filteredTasks:[],
      selectedOption:"All"
    }},
    methods:{
      reset(){
        this.filteredTasks=[...this.tasks]
        this.selectedOption="All"
      },
      addTask(task){
        if (this.tasks.length===0){
          task['id']=1
        }else{
          task['id']=this.tasks[this.tasks.length-1].id+1
        }
        this.tasks.push(task)
        this.reset()
        
      },
      deleteTask(id){
          this.tasks=this.tasks.filter(item=>item.id!==id)
          this.reset()
        },
      editTask(task){
        this.tasks.forEach(item=>{
          if (item.id===task.id){
            item.title=task.data.title
            item.desc=task.data.desc
            item.status=task.data.status
          }
        })
        this.reset()
        },
      deleteAll(){
        this.tasks.splice(0,this.tasks.length)
        this.reset()
      },
      onChange(event){
        this.selectedOption=event.target.value
        if (this.selectedOption==="All"){
        this.filteredTasks=[...this.tasks]
      }else{
        this.filteredTasks=this.tasks.filter((task)=>task.status===this.selectedOption)
        this.filteredTasks=this.filteredTasks.sort(this.compare)
      }
      },compare(a,b){
          const titleA = a.title.toUpperCase();
          const titleB = b.title.toUpperCase();
          let comparison = 0;
          if (titleA > titleB) {
            comparison = 1;
          } else if (titleA < titleB) {
            comparison = -1;
          }
          return comparison;
      }
    },
    created(){
      this.filteredTasks=[...this.tasks]
    }
    ,
  components: {
    'cmp-new-task':NewTask,
    'cmp-task':Tasks
  }
}
</script>

<style>
</style>
