<script>
    import { createEventDispatcher } from 'svelte';
    import { todos } from './stores/TodoStore.js';

    const dispatch = createEventDispatcher();

    let todoId = 4;
    let todoTitle = '';

    function addTodo() {
        todos.update((todos) => [
            ...todos,
            {
                id: todoId++,
                title: todoTitle,
                isComplete: false,
                isEditing: false,
            },
        ]);

        todoTitle = '';
    }
</script>

<form action="#" on:submit|preventDefault={addTodo}>
    <input type="text" class="todo-input" placeholder="What do you need to do?" bind:value={todoTitle} />
</form>
