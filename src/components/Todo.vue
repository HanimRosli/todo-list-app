<template>
<li class="d-flex align-items-center list-group-item">
    <button class="btn border-0 flex-grow-1 text-left shadow-none" :class="{ completed }" @click="$emit('on-toggle')" v-if="!isEditing">
        <span>{{ description }}</span>
    </button>
    <form v-else class="flex-grow-1" @submit.prevent="finishEditing()">
        <input type="text" class="form-control" v-model="newTodoDescription" @blur="finishEditing()" ref="newTodo" />
    </form>
    <b-button @click="startEditing()" variant="dark">
        <span class="fa fa-edit">Edit</span>
    </b-button>
    <b-button @click="$emit('on-delete')" variant="danger">
        <span class="fa fa-trash">Delete</span>
    </b-button>
</li>
</template>

<script>
export default {
    data() {
        return {
            isEditing: false,
            newTodoDescription: ""
        };
    },
    props: {
        description: String,
        completed: Boolean
    },
    methods: {
        startEditing() {
            if (this.isEditing) {
                this.finishEditing();
            } else {
                this.newTodoDescription = this.description;
                this.isEditing = true;
                this.$nextTick(() => this.$refs.newTodo.focus());
            }
        },
        finishEditing() {
            this.isEditing = false;
            this.$emit("on-edit", this.newTodoDescription);
        }
    }
};
</script>

<style lang="scss" scoped>
.completed {
    text-decoration: line-through;
}
</style>
