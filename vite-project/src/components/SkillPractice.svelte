<!-- SkillPractice.svelte -->
<script>
    export let skillPractice;
</script>

<div class="skill-practice">
    <h3>Skill Practice</h3>
    <label for="skill">Time spent practicing a skill (hours):</label>
    <input type="number" id="skill" bind:value={skillPractice} min="0" max="24">
</div>

<style>
    input[type="number"] {
        width: 100%;
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 8px;
        margin-top: 0.25rem;
    }
</style>
