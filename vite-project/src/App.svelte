<script>
    // Mock user data
    let user = {
        name: "Nishit Grover",
        startDate: new Date("2024-09-04"),
        activeDays: 0, 
    };
    
    let currentDate = new Date();
    let startDate = new Date("2024-09-04");
    
    // Calculate days active
    let daysActive = Math.ceil((currentDate.getTime() - startDate.getTime()) / (1000 * 60 * 60 * 24));
    
    // Activities
    let feelings = [
    { mood: "Happy", checked: false },
    { mood: "Relaxed", checked: false },
    { mood: "Energized", checked: false },
    { mood: "Motivated", checked: false },
    { mood: "Grateful", checked: false },
    { mood: "Focused", checked: false },
    { mood: "Anxious", checked: false },
    { mood: "Sad", checked: false },
    { mood: "Stressed", checked: false },
    { mood: "Excited", checked: false },
    { mood: "Calm", checked: false },
    { mood: "Frustrated", checked: false }
    ];

    let image = "";  // Placeholder for image uploads
    let yogaDuration = 0;  // Duration for yoga
    let waterIntake = 0;  // Number of glasses of water
    let dailyReflection = "";  // Text input for journaling
    let skillPractice = 0;  // Number input for time spent practicing a skill
    
    // Function to handle saving
    function saveEntries() {
        alert("Entries saved successfully!");
    }
</script>

<main>
    <!-- User Information -->
    <section class="user-info">
        <h1>Hello, {user.name}</h1>
        <p class="date">Date: {currentDate.toDateString()}</p>
        <p> User started on {user.startDate.toDateString()}</p>
        <p class="active-days">Days Active: {daysActive} </p>
    </section>

    <!-- Activity Tracking -->
    <section class="activity-tracking">
        <h2>Track your activities</h2>
        
        <!-- Feelings (Checkboxes) -->
        <div class="feelings">
            <h3>How are you feeling today?</h3>
            {#each feelings as feeling, index}
                <label>
                    <input type="checkbox" bind:checked={feelings[index].checked}>
                    {feeling.mood}
                </label>
            {/each}
        </div>
        
        <!-- Image Upload -->
        <div class="image-upload and caption entry">
            <h3>Upload a picture and write a caption</h3>
            <input type="file" bind:value={image} accept="image/*">
            <textarea bind:value={dailyReflection} placeholder="Talk something about the picture"></textarea>
        </div>
        
        <!-- Yoga Duration -->
        <div class="yoga-duration">
            <h3>Did you practice yoga today?</h3>
            <label for="yoga">Yoga Duration (minutes):</label>
            <input type="number" id="yoga" bind:value={yogaDuration} min="0">
        </div>

        <!-- Water Intake -->
        <div class="water-intake">
            <h3>Water Intake</h3>
            <label for="water">Glasses of water:</label>
            <input type="number" id="water" bind:value={waterIntake} min="0">
        </div>

        <!-- Daily Reflection -->
        <div class="daily-reflection">
            <h3>Today's Reflection</h3>
            <textarea bind:value={dailyReflection} placeholder="What did you do today?"></textarea>
        </div>

        <!-- Skill Practice -->
        <div class="skill-practice">
            <h3>Skill Practice</h3>
            <label for="skill">Time spent practicing a skill (hours):</label>
            <input type="number" id="skill" bind:value={skillPractice} min="0">
        </div>

        <!-- Save Button -->
        <button on:click={saveEntries}>Save</button>
    </section>
</main>

<style>
    :global(body) {
        background-color: #f0f4f8;
        font-family: 'Arial', sans-serif;
        color: #333;
        margin: 0;
        padding: 0;
    }

    main {
        max-width: 800px;
        margin: 2rem auto;
        padding: 2rem;
        background-color: #fff;
        border-radius: 12px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }

    section {
        margin-bottom: 2rem;
        padding-bottom: 1.5rem;
        border-bottom: 1px solid #eee;
    }

    h1, h2, h3 {
        color: #6200ee;
        margin-bottom: 0.5rem;
    }

    h1 {
        font-size: 2rem;
    }

    h2 {
        font-size: 1.5rem;
    }

    h3 {
        font-size: 1.2rem;
    }

    .user-info {
        text-align: center;
    }

    .user-info .date, .user-info .active-days {
        font-size: 1.1rem;
        margin-top: 0.5rem;
    }

    .activity-tracking label, .activity-tracking textarea {
        display: block;
        margin-top: 0.5rem;
        margin-bottom: 1rem;
    }

    input[type="file"], input[type="number"], textarea {
        width: 100%;
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 8px;
        margin-top: 0.25rem;
    }

    textarea {
        min-height: 100px;
    }

    button {
        background-color: #6200ee;
        color: white;
        padding: 0.75rem 1.5rem;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        font-size: 1rem;
        transition: background-color 0.3s;
    }

    button:hover {
        background-color: #3700b3;
    }

    .feelings label {
        display: inline-block;
        margin-right: 1rem;
    }

    input[type="checkbox"] {
        margin-right: 0.5rem;
    }
</style>