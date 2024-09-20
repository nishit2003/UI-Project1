<!-- Feelings.svelte -->
<script>
    export let feelings;
</script>

<div class="feelings">
    <h3>How are you feeling today?</h3>
    {#each feelings as feeling, index}
        <label>
            <input type="checkbox" bind:checked={feelings[index].checked}>
            {feeling.mood}
        </label>
    {/each}
</div>

<style>
    .feelings label {
        display: inline-block;
        margin-right: 1rem;
    }

    input[type="checkbox"] {
        margin-right: 0.5rem;
    }
</style>
