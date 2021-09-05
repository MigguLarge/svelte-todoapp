<script>
    import { onMount } from 'svelte';

    import { v4 } from 'uuid';

    let todoText = '';
    let todos = [];

    onMount(() => {
        const exsistingTodos = localStorage.getItem('todos');
        if (exsistingTodos !== null) {
            todos = JSON.parse(exsistingTodos);
        } else {
            todos = [];
        }
    });

    function addTodo(title) {
        todos = [...todos, { title: title, id: v4(), completed: false }];
        localStorage.setItem('todos', JSON.stringify(todos));
        todoText = '';
    }

    function deleteTodo(id) {
        todos = todos.filter((item) => item.id !== id);
        localStorage.setItem('todos', JSON.stringify(todos));
    }
</script>

<div class="todo-container">
    {#if todos.length === 0}
        <h1 class="empty-todo">There is no todo rn!</h1>
    {/if}

    <ul class="todos">
        {#each todos as todo}
            <li class="todos__item" id={todo.completed ? 'completed' : ''}>
                <span
                    class="item__title"
                    on:click={() => (todo.completed = !todo.completed)}
                    >{todo.title}</span
                >
                <button class="item__delete" on:click={deleteTodo(todo.id)}
                    >✗</button
                >
            </li>
        {/each}
    </ul>

    <form class="todo-form" on:submit|preventDefault={addTodo(todoText)}>
        <input
            type="text"
            placeholder="Type some todos"
            bind:value={todoText}
            class="todo-form__input"
        />
        <input type="submit" value="Add todo" class="todo-form__submit" />
    </form>
</div>

<style>
    @keyframes strike {
        0% {
            width: 0;
        }
        100% {
            width: 100%;
        }
    }

    #completed {
        position: relative;
    }

    #completed::after {
        content: '';
        position: absolute;
        top: 50%;
        left: 0;
        width: 100%;
        height: 1px;
        background: var(--warn);
        animation: strike 0.5s ease-out;
    }

    .todo-container {
        width: max-content;
        background: var(--bg);
        border: 1px solid var(--fg);
        padding: 5em;
        margin-top: 5em;
        filter: drop-shadow(1em 1em var(--fg));
    }

    .todos {
        list-style: square;
    }

    .todos__item {
        border-bottom: 1px solid var(--fg);
        margin-bottom: 0.5em;
    }

    .item__title {
        cursor: pointer;
    }

    .item__delete {
        background: transparent;
        border: none;
        color: var(--warn);
        cursor: pointer;
        margin-left: 0.5em;
    }

    .todo-form {
        display: flex;
    }

    .todo-form__input {
        border: 1px solid var(--fg);
        border-radius: 0;
        padding: 0.5em;
        background: var(--bg);
        color: var(--fg);
    }

    .todo-form__submit {
        border: none;
        border-radius: 0;
        padding: 0.5em;
        background: var(--fg);
        color: var(--bg);
    }
</style>
