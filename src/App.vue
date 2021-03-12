<template>
    <NewTodoForm :loadDataFromServer="loadDataFromServer" :serverUrl="serverUrl" />
    <TodoList :currentTodoList="currentTodoList" />
</template>

<script>
import NewTodoForm from "./components/NewTodoForm.vue";
import TodoList from "./components/TodoList.vue";

export default {
    name: "app",
    components: {
        NewTodoForm,
        TodoList,
    },
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
