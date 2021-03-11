<template>
    <div class="form-check todo_item mt-5 me-auto ms-auto col-auto">
        <input type="checkbox" class="form-check-input" />
        <label class="form-check-label">{{newTodoText}}</label>
    </div>
</template>

<script>
export default {
    name: "todo-item",
    props: {
        newTodoText: String,
    },
};
</script>
