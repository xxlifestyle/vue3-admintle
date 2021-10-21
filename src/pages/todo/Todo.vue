<template lang="ru">
    <div>
        <section class="content-header">
    <div class="container-fluid">
        <div class="row mb-2">
            <div class="col-sm-6 row">
                <h1>Todo List</h1> 
                <button  @click="clearTodoPosts" class="clear-list_btn">Clear list</button>
            </div>
            <div class="col-sm-6">
                <ol class="breadcrumb float-sm-right">
                    <li class="breadcrumb-item"><a href="#">Home</a></li>
                    <li class="breadcrumb-item active">Blank Page</li>
                </ol>
            </div>
        </div>
    </div>
</section>
<section class="content">
    <div class="container-fluid">
        <div class="card">
            
           
            <div class="card-body">
               <input @keyup.enter="pushListToTodoPosts" v-model='inputHolder' placeholder="Enter your task" class='col-sm-9'>
               <button @click="pushListToTodoPosts" class='col-sm-1'>Create list</button>
              <ul class=" todo-list">
                  <li class='todo-list_item '  v-for="(post, index) in todoPosts"
          ><span>{{post}}</span>
                      <button @click="todoPosts.splice(index, 1)" class="col-sm-1 clear-list_item__btn">Delete</button>
                  </li> 
              </ul>
            </div>
            <div class="card-footer">Footer</div>
           
        </div>
    </div>
</section>
    </div>

</template>
<script>
export default {
    name: "Todo",
    data(){
        return{
            inputHolder:null,
            todoPosts : [],
        }
    },
    methods:{
        pushListToTodoPosts(){
            if ((this.inputHolder === null) ||(this.inputHolder === '')) { return } 
                this.todoPosts.push(this.inputHolder)
                this.inputHolder = ''
            
            
        },
        clearTodoPosts(){
            this.todoPosts = []
        },
    },

}
</script>
<style>

.todo-list li{
    margin-top:15px ;
    display: flex;
    width: 83%;
    justify-content: space-between;
}
.clear-list_btn{
    margin-left:15px;
    background-color: red ;
    color: white;
    border: 1px solid white;
}

.clear-list_item__btn{
    
    background-color: red ;
    color: white;
    border: 1px solid white;
}

</style>