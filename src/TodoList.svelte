<script>
    import { fade, fly, scale } from 'svelte/transition';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let filteredTodos;
    export let remainingTodos;
    export let currentFilter;

    function checkAllTodos() {
        dispatch('checkAllTodosDispatched');
    }

    function clearCompleted() {
        dispatch('clearCompletedDispatched');
    }

    function deleteTodo(id) {
        dispatch('deleteTodoDispatched', {
            id,
        });
    }

    function updateFilter(filter) {
        dispatch('updateFilterDispatched', {
            filter,
        });
    }

    function editTodo(todo) {
        dispatch('editTodoDispatched', {
            todo,
        });
    }

    function doneEdit(todo) {
        dispatch('doneEditDispatched', {
            todo,
        });
    }

    function doneEditKeydown(event, todo) {
        dispatch('doneEditKeydownDispatched', {
            key: event.key,
            todo,
        });
    }
</script>

<div>
    <ul class="todo-list">
        {#each filteredTodos as todo (todo.id)}
            <li class="todo-item-container" in:scale out:fade>
                <div class="todo-item">
                    <input type="checkbox" bind:checked={todo.isComplete} />
                    {#if !todo.isEditing}
                        <span on:dblclick={editTodo(todo)} class="todo-item-label" class:line-through={todo.isComplete}
                            >{todo.title}</span
                        >
                    {:else}
                        <input
                            type="text"
                            class="todo-item-input"
                            bind:value={todo.title}
                            on:blur={doneEdit(todo)}
                            on:keyup={(event) => doneEditKeydown(event, todo)}
                            autofocus
                        />
                    {/if}
                </div>

                <button class="x-button" on:click={deleteTodo(todo.id)}>
                    <svg class="x-button-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </li>
        {/each}
    </ul>

    <div class="check-all-container">
        <div>
            <button class="button" on:click={checkAllTodos}>Check All</button>
        </div>

        <div>
            {#key remainingTodos}
                <span style="display: inline-block" in:fly={{ y: -20 }}>{remainingTodos}</span>
            {/key}
            <span>items remaining</span>
        </div>
    </div>

    <div class="other-buttons-container">
        <div>
            <button
                on:click={() => updateFilter('all')}
                class="button filter-button"
                class:filter-button-active={currentFilter === 'all'}>All</button
            >
            <button
                on:click={() => updateFilter('active')}
                class="button filter-button"
                class:filter-button-active={currentFilter === 'active'}>Active</button
            >
            <button
                on:click={() => updateFilter('completed')}
                class="button filter-button"
                class:filter-button-active={currentFilter === 'completed'}>Completed</button
            >
        </div>
        <div>
            <button class="button" on:click={clearCompleted}>Clear completed</button>
        </div>
    </div>
</div>
