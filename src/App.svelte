<script>
    import './css/style.css';
    import NoTodosContainer from './NoTodosContainer.svelte';
    import TodoForm from './TodoForm.svelte';
    import TodoList from './TodoList.svelte';
    import { todos } from './stores/TodoStore.js';
</script>

<div class="todo-app-container">
    <div class="todo-app">
        <h2>Todo App</h2>
        <TodoForm />

        {#if $todos.length > 0}
            <TodoList />
        {:else}
            <NoTodosContainer />
        {/if}
    </div>
</div>
