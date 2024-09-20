<!-- Reflection.svelte -->
<script>
    export let dailyReflection;
</script>

<div class="daily-reflection">
    <h3>Today's Reflection</h3>
    <textarea bind:value={dailyReflection} placeholder="What did you do today?"></textarea>
</div>

<style>
    textarea {
        width: 100%;
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 8px;
        min-height: 100px;
    }
</style>
