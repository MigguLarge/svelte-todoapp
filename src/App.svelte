<script>
    import Todo from './Todo.svelte';
</script>

<main>
    <Todo />
</main>

<style>
    :global(:root) {
        --fg: #555555;
        --bg: #f6f6f6;
        --warn: #d35d6e;
    }

    :global(body) {
        background: var(--bg);
    }

    * {
        font-family: 'Jetbrains Mono', monospace;
        color: var(--fg);
    }

    main {
        display: flex;
        justify-content: center;
    }
</style>
