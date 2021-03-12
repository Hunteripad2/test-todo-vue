<template>
    <form class="row g-3 justify-content-center mt-5" id="newtodo_form" @change="unlockButton" @submit="addTodo">
        <div class="col-6">
            <input type="text" class="form-control" id="newtodo_form_text" placeholder="Введите новую задачу..." />
        </div>
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3" id="newtodo_form_button" disabled>Добавить задачу</button>
        </div>
    </form>
</template>

<script>
export default {
    name: "new-todo-form",
    props: {
        loadDataFromServer: Function,
        serverUrl: String,
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
    },
};
</script>
