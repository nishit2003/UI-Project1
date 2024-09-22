<script>
  export let AddYes = false;
  export let additionalActivityName = ""; // Activity name entered by the user
  export let inputType = ""; // Type of input (slider, text, number)
  export let activityValue = 0; // Default value for activity

  // Available input types
  let inputTypes = ["Slider", "Text", "Number"];
</script>

<div class="additional-activity">
  <h3>Would you like to add an additional activity?</h3>

  <div class="yes-no-buttons">
    <label>
      <input type="radio" bind:group={AddYes} value={true} /> Yes
    </label>
    <label>
      <input type="radio" bind:group={AddYes} value={false} /> No
    </label>
  </div>

  {#if AddYes}
    <!-- Ask user for the name of the activity -->
    <div class="activity-name">
      <label for="activity">What activity would you like to track?</label>
      <input
        type="text"
        id="activity"
        bind:value={additionalActivityName}
        placeholder="Enter activity name"
      />
    </div>

    <!-- Ask user what type of input they would like to use -->
    <div class="input-type">
      <label for="input-type">Choose the type of input for tracking:</label>
      <select id="input-type" bind:value={inputType}>
        <option value="" disabled>Select input type</option>
        {#each inputTypes as type}
          <option value={type}>{type}</option>
        {/each}
      </select>
    </div>

    <!-- Dynamically show input based on selected input type -->
    {#if inputType === "Slider"}
      <div class="activity-slider">
        <label for="slider">Track your {additionalActivityName}:</label>
        <input
          type="range"
          id="slider"
          bind:value={activityValue}
          min="0"
          max="100"
          step="5"
          style="width: 80%"
        />
        <p>
          You've tracked {activityValue} units of {additionalActivityName} today.
        </p>
      </div>
    {/if}

    {#if inputType === "Text"}
      <div class="activity-text">
        <label for="text">Enter details for {additionalActivityName}:</label>
        <input
          type="text"
          id="text"
          bind:value={activityValue}
          placeholder="Enter details"
        />
        <p>You've entered: {activityValue}</p>
      </div>
    {/if}

    {#if inputType === "Number"}
      <div class="activity-number">
        <label for="number">Enter a number for {additionalActivityName}:</label>
        <input type="number" id="number" bind:value={activityValue} min="0" />
        <p>
          You've entered: {activityValue} units of {additionalActivityName}.
        </p>
      </div>
    {/if}
  {/if}
</div>

<style>
  .additional-activity {
    margin-bottom: 1.5rem;
  }

  /* Flexbox for evenly spaced Yes/No buttons */
  .yes-no-buttons {
    display: flex;
    justify-content: space-between;
    margin: 1rem 0;
  }

  .yes-no-buttons label {
    flex: 1;
    text-align: center;
    margin: 0 5px;
    cursor: pointer;
    background-color: var(--primary-color);
    padding: 0.5rem;
    border-radius: 8px;
    color: white;
    transition: background 0.3s ease;
  }

  .yes-no-buttons label:hover {
    background-color: var(--primary-color-dark);
  }

  /* Activity input type */
  .activity-name,
  .input-type {
    margin-top: 1rem;
  }

  .activity-name input,
  .input-type select {
    width: 100%;
    padding: 0.5rem;
    margin-top: 0.5rem;
  }

  .activity-slider,
  .activity-text,
  .activity-number {
    margin-top: 1rem;
  }
</style>
