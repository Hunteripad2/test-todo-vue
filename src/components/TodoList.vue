<template>
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
export default {
    name: "todo-list",
    props: {
        currentTodoList: Array,
    },
};
</script>
