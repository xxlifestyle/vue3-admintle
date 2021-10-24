<template lang="ru">
    <div>
        <section class="content-header">
    <div class="container-fluid">
        <div class="row mb-2">
            <div class="col-sm-6 row">
                <h1>Todo List</h1> 
                <h6 v-if="todoPosts.length == 0"  class="completed">Завершено</h6>
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
            <div class='list-form'>
            <input @keyup.enter="pushListToTodoPosts" v-model='inputListHolder' placeholder="Enter your list" class='col-sm-9 list-input'>
            <button @click="pushListToTodoPosts" class='col-sm-1 list-input_button'>Create list</button>
            </div>
            <ul class=" todo-list">
                <li class='todo-list_item' v-for="(post, index) in todoPosts">
                    <div class='todo-list_item__content'>
                    <span  class="post-name">{{post.name }}</span>  
                    <button @click="clearTodoPosts(index,post.id)" class=" clear-list_btn">Delete</button>
                    </div>
                    <ul v-if="todoPosts.length !== 0" class=" task-list">
                        <li class='task-list_item '  v-for="(task, index) in todoPosts[index].todo_list">
                            <div class='task-list_item__content'>
                            <span>{{ task.name }}</span>
                            <img src='@/assets/crossflat.svg' @click="clearTodoTasks(index,task.id)" class=" clear-list_item__btn"/>
                            </div>
                        </li> 
                        <div class="new-task_form">
                            <input v-model='inputTaskHolder' placeholder="Enter your task" class=' task-input'/> 
                            <button @click="pushListToTaskPosts(post.id,index)" class=''>Create task</button>
                        </div>
                    </ul>
                </li> 
                    
            </ul>
        </div>
            <div class="card-footer"></div>
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
            inputListHolder: null,
            inputTaskHolder: null,
            todoPosts: [],
            taskPosts: []
        };
    },
    methods: {
        pushListToTaskPosts(id, index) {
            if (this.inputTaskHolder === null || this.inputTaskHolder === '') {
                return;
            }
            axios
                .post(
                    'http://dev1.itpw.ru:8003/todo/tasks/',
                    {
                        name: this.inputTaskHolder,
                        todo_list: id
                    },
                    {
                        headers: {
                            Authorization:
                                'Bearer ' + window.localStorage.getItem('token')
                        }
                    }
                )
                .then((response) => {
                    setTimeout(this.renderPosts, 500);
                });
            this.inputTaskHolder = '';
        },
        clearTodoTasks(index, id) {
            axios.delete('http://dev1.itpw.ru:8003/todo/tasks/' + id, {
                headers: {
                    Authorization:
                        'Bearer ' + window.localStorage.getItem('token')
                }
            });

            setTimeout(this.renderPosts, 500);
        },

        pushListToTodoPosts() {
            if (this.inputListHolder === null || this.inputListHolder === '') {
                return;
            }
            axios
                .post(
                    'http://dev1.itpw.ru:8003/todo/lists/',
                    {
                        name: this.inputListHolder
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
            this.inputListHolder = '';
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
                        axios
                            .get(
                                'http://dev1.itpw.ru:8003/todo/tasks/?todo_list=' +
                                    response.data[i].id,
                                {
                                    headers: {
                                        Authorization:
                                            'Bearer ' +
                                            window.localStorage.getItem('token')
                                    }
                                }
                            )
                            .then((resp) => {
                                this.todoPosts.push({
                                    name: response.data[i].name,
                                    id: response.data[i].id,
                                    todo_list: resp.data
                                });
                            });
                    }
                });
        }
    },
    mounted() {
        setTimeout(this.renderPosts, 500);
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

.todo-list_item {
    display: flex;
    flex-direction: column;
}
.post-name {
    font-size: 24px;
    font-weight: 700;
}
.todo-list_item__content {
    display: flex;
    flex-direction: row;
    widows: 100%;
    justify-content: space-between;
}
.task-list li {
    margin-top: 15px;
    display: flex;
    width: 83%;
    justify-content: space-between;
    flex-direction: column;
    border: 1px double gray;
    padding: 5px;
}
.clear-list_btn {
    margin-left: 15px;
    background-color: red;
    color: white;
    border: 1px solid white;
    width: 80px;
}

.task-list_item {
    width: 100%;
}

.task-list_item span {
    margin-left: 5px;
}

.task-input {
    margin-right: 10px;
}

.task-list_item__content {
    display: flex;
    flex-direction: row;
    width: 100%;
    justify-content: space-between;
}

.new-task_form {
    width: 100%;
    display: flex;
    flex-direction: row;
    margin-top: 10px;
}
.new-task_form button {
    height: 30px;
    width: 100px;
}

.clear-list_item__btn {
    color: white;
    border: 1px solid white;
    height: 30px;
}

.completed {
    color: rgb(76, 148, 4);
    font-weight: 700;
}
.task-input {
    width: 75.3%;
    border: 1px solid black;
}
.card-body {
    display: flex;
    flex-direction: column;
}
.list-form {
    width: 100%;
}
.list-input {
    height: 30px;
}
.list-input_button {
    height: 30px;
    margin-left: 10px;
}
</style>
