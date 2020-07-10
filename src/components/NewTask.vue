<template>
    <div class="container">
        <div class="row">
            <div class="col-sm">
                <h3>Add task</h3>
                <form>
                    <div class="form-group">
                        <label for="title">Title:</label>
                        <input id="title" class="form-control" v-model="title" name="title" type="text" required>
                    </div>
                    <div class="form-group">
                        <label for="desc">Description: </label>
                        <textarea row="5" class="form-control" v-model="desc" id="desc" name="desc"></textarea>
                    </div>
                    <button class="btn btn-primary" @click="create" :disabled="title=='' || desc==''">Create</button>&nbsp;
                    <button class="btn btn-secondary" @click="cancel" :disabled="title=='' || desc==''">Cancel</button>
                </form> 
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        data:function(){
            return{
                title:"",
                desc:""
            }
            },
        methods:{
            create(){
                this.$emit('create',{title:this.title,desc:this.desc,status:"Pending"})
                this.cancel()
                },
            cancel(){
                this.title=""
                this.desc=""
                }
        }
    }
</script>

<style>

</style>