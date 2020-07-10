<template>
    <div class="container">
        <div v-if="!editable" class="card-body">
                    <h5 class="card-title">{{task.title}}</h5>
                    <p class="card-text">Description: {{task.desc}}</p>
                    <p :style="{color:activeColor}" class="card-text">Progress  : {{task.status}}</p>
                    <button class="btn btn-warning" @click="edit" >Edit</button>&nbsp;
                    <button class="btn btn-outline-danger" @click="del">Delete</button>
                    <br>
        </div>
        <div v-else>
            <div class="row">
                <div class="col-md">
                    <form>
                        <div class="form-group">
                            <label for="title1">Title:</label>
                            <input class="form-control" id="title1" v-model="title" type="text">
                        </div>
                        
                            <label for="desc1">Description: </label>
                            <input type="textarea" id="desc1" class="form-control" v-model="desc">
                        
                        <div class="form-group">
                        <label for="progress1">Progress</label>
                        <select id="progress1" class="form-control" v-model="status">
                            <option v-for="status in statuses" :key="status">{{status}}</option>
                        </select>
                        </div>
                        <button class="btn btn-success" @click="editConfirm">Confirm</button>&nbsp;
                        <button class="btn btn-info" @click="editCancel">Cancel</button>
                    </form>
                    <br>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name:"Tasks",
    props:['task','index'],
    data(){return{
        editable:false,
        title:this.task.title,
        desc:this.task.desc,
        status:this.task.status,
        statuses:['Pending','In Progress','Completed'],
        colorBind:{'Pending':'brown','In Progress':'blue','Completed':'green'},
        activeColor:'blue'
    }},
    created(){
        this.activeColor=this.colorBind[this.task.status]
    },
    updated(){
        this.activeColor=this.colorBind[this.task.status]
    },
    methods:{
        edit(){
            console.log("inside edit")
            this.editable=true
        },
        del(){
            console.log("inside del")
            this.$emit('deleteTask',this.task.id)
            this.editable=false
        },
        editConfirm(){
            this.$emit('editConfirm',{data:{title:this.title,desc:this.desc,status:this.status},id:this.task.id})
            this.editable=false
        },
        editCancel(){
            this.title=this.task.title
            this.desc=this.task.desc
            this.status=this.task.status
            this.editable=false
        }
    }
    }
</script>

<style>

</style>