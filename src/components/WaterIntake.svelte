<script>
    import { tweened } from 'svelte/motion';
    import { cubicOut } from 'svelte/easing';

    const progress = tweened(0, {
        duration: 400,
        easing: cubicOut
    });

    export let waterIntake = 0; // Add this to handle two-way binding

    $: progress.set(waterIntake / 4);  // Update progress when waterIntake changes

    export function setWaterIntake(litres) {
        waterIntake = litres;
        progress.set(litres / 4);
    }
</script>

<div class="water-intake">
    <h3>How much water did you drink today?</h3>
    <progress value={$progress} max="1"></progress>

    <div class="water-buttons">
        <button on:click={() => setWaterIntake(0)}> 0L </button>
        <button on:click={() => setWaterIntake(1)}> 1L </button>
        <button on:click={() => setWaterIntake(2)}> 2L </button>
        <button on:click={() => setWaterIntake(3)}> 3L </button>
        <button on:click={() => setWaterIntake(4)}> 4L </button>
    </div>
</div>

<style>
    .water-buttons {
        display: flex;
        justify-content: space-between; /* Evenly space the buttons */
        margin-top: 1rem; /* Optional: Add some margin above the button row */
    }

    .water-buttons button {
        flex: 1; 
        margin: 0 25px; /* Add a small margin between buttons */
        padding: 0.5rem;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        background-color: var(--primary-color);
        color: white;
        transition: background 0.3s ease;
    }

    .water-buttons button:hover {
        background-color: var(--primary-color-dark);
    }

    progress {
        display: block;
        width: 100%;
        height: 20px;
        margin-bottom: 1rem;
    }
</style>
