<script>
    import { tweened } from 'svelte/motion';
    import { cubicOut } from 'svelte/easing';

    const progress = tweened(0, {
        duration: 400,
        easing: cubicOut
    });

    export let waterIntake = 0; // Add this to handle two-way binding

    $: progress.set(waterIntake);  // Update progress when waterIntake changes

    export function setWaterIntake(percentage) {
        waterIntake = percentage;
        progress.set(percentage);
    }
</script>

<div class="water-intake">
    <h3>How much water did you drink today?</h3>
    <progress value={$progress} max="1"></progress>

    <div class="water-buttons">
        <button on:click={() => setWaterIntake(0)}> 0L </button>
        <button on:click={() => setWaterIntake(0.25)}> 1L </button>
        <button on:click={() => setWaterIntake(0.5)}> 2L </button>
        <button on:click={() => setWaterIntake(0.75)}> 3L </button>
        <button on:click={() => setWaterIntake(1)}> 4L </button>
    </div>
</div>

<style>
    .water-buttons button {
        margin-right: 10px;
    }

    progress {
        display: block;
        width: 100%;
        height: 20px;
        margin-bottom: 1rem;
    }
</style>
