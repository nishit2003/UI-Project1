<!-- YogaDuration.svelte -->
<script>
    export let didYoga;
    export let yogaDuration;
</script>

<div class="yoga-duration">
    <h3>Did you practice yoga today?</h3>
    <label>
        <input type="radio" bind:group={didYoga} value={true}> Yes
    </label>
    <label>
        <input type="radio" bind:group={didYoga} value={false}> No
    </label>

    {#if didYoga}
        <div class="yoga-slider">
            <label for="yoga">Yoga Duration (minutes):</label>
            <input type="range" id="yoga" bind:value={yogaDuration} min="0" max="100" step="5" style="width: 80%">
            <p>You've done {yogaDuration} minutes of yoga today.</p>
        </div>
    {/if}
</div>

<style>
    .yoga-duration {
        margin-bottom: 1.5rem;
    }
</style>
