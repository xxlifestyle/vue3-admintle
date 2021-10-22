<template lang="ru">
    <div>
        <section class="content-header">
    <div class="container-fluid">
        <div class="row mb-2">
            <div class="col-sm-6 row">
                <h1>Todo List</h1> 
                <h6 v-if="todoPosts.length == 0"  class="completed">Завершено</h6>
                <h6 v-if="todoPosts.length == 0"  class="completed">{{completedCount}}</h6>
                
                <!-- <button  @click="clearTodoPosts" class="clear-list_btn">Clear list</button> -->
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
          ><span>{{post.name }}</span>
                      <button @click="clearTodoPosts(index,post.id)" class="col-sm-1 clear-list_item__btn">Delete</button>
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
import axios from 'axios';
export default {
    name: 'Todo',
    data() {
        return {
            inputHolder: null,
            todoPosts: []
        };
    },
    methods: {
        pushListToTodoPosts() {
            if (this.inputHolder === null || this.inputHolder === '') {
                return;
            }
            axios
                .post(
                    'http://dev1.itpw.ru:8003/todo/lists/',
                    {
                        name: this.inputHolder
                    },
                    {
                        headers: {
                            Authorization:
                                'Bearer ' + window.localStorage.getItem('token')
                        }
                    }
                )
                .then((response) => {
                    this.todoPosts.push({
                        name: response.data.name,
                        id: response.data.id
                    });
                });
            this.inputHolder = '';
        },

        clearTodoPosts(index, id) {
            axios.delete('http://dev1.itpw.ru:8003/todo/lists/' + id, {
                headers: {
                    Authorization:
                        'Bearer ' + window.localStorage.getItem('token')
                }
            });

            this.todoPosts.splice(index, 1);
        },
        renderPosts() {
            this.todoPosts = [];
            axios
                .get('http://dev1.itpw.ru:8003/todo/lists/', {
                    headers: {
                        Authorization:
                            'Bearer ' + window.localStorage.getItem('token')
                    }
                })
                .then((response) => {
                    for (let i = 0; i < response.data.length; i++) {
                        this.todoPosts.push({
                            name: response.data[i].name,
                            id: response.data[i].id
                        });
                        console.log(response.data);
                    }
                });
        }
    },
    beforeMount() {
        this.renderPosts();
        console.log(this.todoPosts);
    }
};
</script>
<style>
.todo-list li {
    margin-top: 15px;
    display: flex;
    width: 83%;
    justify-content: space-between;
}
.clear-list_btn {
    margin-left: 15px;
    background-color: red;
    color: white;
    border: 1px solid white;
}

.clear-list_item__btn {
    background-color: red;
    color: white;
    border: 1px solid white;
}

.completed {
    color: rgb(76, 148, 4);
    font-weight: 700;
}
</style>
