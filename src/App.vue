<template>
    <form class="row g-3 justify-content-center mt-5" id="newtodo_form" @change="unlockButton" @submit="addTodo">
        <div class="col-6">
            <input type="text" class="form-control" id="newtodo_form_text" placeholder="Введите новую задачу..." />
        </div>
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3" id="newtodo_form_button" disabled>Добавить задачу</button>
        </div>
    </form>
    <div class="container">
        <div class="row" id="todo_list" v-if="currentTodoList">
            <div class="form-check todo_item mt-5 me-auto ms-auto col-auto" v-for="todo in currentTodoList" v-bind:key="todo.id">
                <input type="checkbox" class="form-check-input" />
                <label class="form-check-label">{{ todo.text }}</label>
            </div>
        </div>
    </div>
</template>

<script>
//import { TodoItem } from "@/components/TodoItem";
//import TodoList from "@/components/TodoList.vue";

export default {
    name: "app",
    //components: {
    //	TodoItem,
    //	TodoList,
    //},
    computed: {
        serverUrl() {
            const hostname = window.location.hostname;
            const isLocalClient = hostname === "localhost" || hostname === "127.0.0.1" || hostname === "0.0.0.0";

            const localServer = "http://localhost:3001/";
            const herokuServer = "https://sleepy-waters-99292.herokuapp.com/";

            return isLocalClient ? localServer : herokuServer;
        },
    },
    data() {
        return {
            currentTodoList: [],
        };
    },
    methods: {
        unlockButton() {
            const newTodoText = document.querySelector("#newtodo_form_text").value;
            const newTodoButton = document.querySelector("#newtodo_form_button");

            if (newTodoText) {
                newTodoButton.removeAttribute("disabled");
            } else newTodoButton.setAttribute("disabled", "");
        },

        addTodo(event) {
            event.preventDefault();

            const newTodoText = document.querySelector("#newtodo_form_text").value;
            const newTodoButton = document.querySelector("#newtodo_form_button");
            const todoList = document.querySelector("#todo_list");
            const todoItemList = document.querySelectorAll(".todo_item");
            const newTodo = { id: todoItemList.length, text: newTodoText };

            if (todoItemList.length % 2 === 0) {
                todoList.insertAdjacentHTML("beforeend", "<div class='w-100'></div>");
            }

            fetch(this.serverUrl + "todoData", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(newTodo),
            }).then(this.loadDataFromServer());

            document.querySelector("#newtodo_form_text").value = "";
            newTodoButton.setAttribute("disabled", "");
        },

        loadDataFromServer() {
            fetch(this.serverUrl + "todoData")
                .then((response) => {
                    return response.json();
                })
                .then((data) => {
                    this.currentTodoList = data;
                });
        },
    },
    mounted() {
        this.loadDataFromServer();
    },
};
</script>
