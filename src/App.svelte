<script>
    import Login from './components/Login.svelte';
    import Feelings from './components/Feelings.svelte';
    import ImageUpload from './components/ImageUpload.svelte';
    import Reflection from './components/Reflection.svelte';
    import SkillPractice from './components/SkillPractice.svelte';
    import YogaDuration from './components/YogaDuration.svelte';
    import UserInfo from './components/UserInfo.svelte';
    import WaterIntake from './components/WaterIntake.svelte';
    import ProductivityChart from './components/ProductivityChart.svelte';  // Import the chart component
    import Progress from './components/Progress.svelte';  // Import new Progress component

    let loggedIn = false;  // Track if user is logged in
    let user = { username: '', startDate: new Date() };
    let currentDate = new Date();  // Current date for user information
    let selectedDate = new Date();  // Default is today's date
    let daysActive = 1;

    // Journal Data
    let feelings = [
        { mood: 'Happy', checked: false },
        { mood: 'Sad', checked: false },
        { mood: 'Excited', checked: false },
        { mood: 'Anxious', checked: false },
        { mood: 'Calm', checked: false },
        { mood: 'Frustrated', checked: false },
        { mood: 'Grateful', checked: false },
        { mood: 'Lonely', checked: false },
        { mood: 'Motivated', checked: false },
        { mood: 'Tired', checked: false },
        { mood: 'Confident', checked: false },
        { mood: 'Bored', checked: false }
    ];
    let image = '';
    let dailyReflection = '';
    let skillPractice = 0;
    let didYoga = false;
    let yogaDuration = 0;
    let waterIntake = 0;
    let skill = '';

    // Productivity data for chart
    let productivityData = [skillPractice * 60 , yogaDuration / 60 , waterIntake * 80];

    // Progress tracking logic
    let skillPracticeGoal = 5; // Example: 60 minutes skill practice per day
    let yogaGoal = 100; // Example: 30 minutes yoga per day
    let waterGoal = 4; // Example: 2000 ml water intake per day

    let skillPracticeProgress = (skillPractice / skillPracticeGoal) * 100;
    let yogaProgress = (yogaDuration / yogaGoal) * 4;
    let waterIntakeProgress = ((waterIntake) / waterGoal) * 4;

    // Helper function to format date as YYYY-MM-DD for input value
    const formatDateForInput = (date) => {
        return date.toISOString().split('T')[0];
    };

    // Handle login success and retrieve stored data
    const onLoginSuccess = (event) => {
        user.username = event.detail.username;  // Assign username after login
        user.Name = event.detail.Name;  // Assign username after login
        loggedIn = true;
        retrieveJournalData();  

        setTimeout(() => {
            skill = prompt(`What skill would you like to work on today, ${user.Name}?`) || 'Skill';  // Fallback to 'Skill' if input is empty
    }, 100);
    };

    // Update journal data based on selected date
    const onDateChange = (event) => {
        selectedDate = new Date(event.target.value);
        retrieveJournalData();  // Load journal data for the newly selected date
    };

    // Save journal data in localStorage for the selected date
    const saveJournalData = () => {
        const journalData = {
            feelings,
            image,
            dailyReflection,
            skillPractice,
            didYoga,
            yogaDuration,
            waterIntake
        };

        // Store the data in localStorage with the user's name and the selected date as key

        const formattedDate = selectedDate.toDateString();
        localStorage.setItem(user.username + '_journal_' + formattedDate, JSON.stringify(journalData));

        // Update productivityData for the pie chart
        productivityData = [skillPractice, yogaDuration / 60, waterIntake];

        // Update progress tracking
        skillPracticeProgress = (skillPractice / skillPracticeGoal) * 100;
        yogaProgress = (yogaDuration / yogaGoal) * 100;
        waterIntakeProgress = (waterIntake / waterGoal) * 100;

        alert('Journal saved successfully for ' + formattedDate + '!');
    };

    // Retrieve journal data from localStorage and populate the fields for the selected date
    const retrieveJournalData = () => {
        const formattedDate = selectedDate.toDateString();
        const savedJournalData = localStorage.getItem(user.username + '_journal_' + formattedDate);

        if (savedJournalData) {
            const journalData = JSON.parse(savedJournalData);

            // Restore all fields with the saved data
            feelings = journalData.feelings || feelings;
            image = journalData.image || '';
            dailyReflection = journalData.dailyReflection || '';
            skillPractice = journalData.skillPractice || 0;
            didYoga = journalData.didYoga || false;
            yogaDuration = journalData.yogaDuration || 0;
            waterIntake = journalData.waterIntake || 0;

            // Update productivityData for the pie chart
            productivityData = [skillPractice, yogaDuration/60, waterIntake];

            // Update progress tracking
            skillPracticeProgress = (skillPractice / skillPracticeGoal) * 100;
            yogaProgress = (yogaDuration / yogaGoal) * 100;
            waterIntakeProgress = (waterIntake / waterGoal);
        } else {
            // If no data for the selected date, reset the fields
            feelings.forEach(feeling => feeling.checked = false);
            image = '';
            dailyReflection = '';
            skillPractice = 0;
            didYoga = false;
            yogaDuration = 0;
            waterIntake = 0;

            // Reset productivityData
            productivityData = [0, 0, 0];

            // Reset progress tracking
            skillPracticeProgress = 0;
            yogaProgress = 0;
            waterIntakeProgress = 0;

        }
    };

    // Function to handle fetching journal data or showing an alert if no data is found
    const handleJournalFetch = () => {
    const formattedDate = selectedDate.toISOString().split('T')[0];  // Format as YYYY-MM-DD
        const savedJournalData = localStorage.getItem(user.username + '_journal_' + formattedDate);

        if (savedJournalData) {
            retrieveJournalData();  // Fetch and populate journal data if it exists
        } else {
            alert(`No journal data found for ${formattedDate}.`);  // Show alert if no data found
        }
    };

</script>

<main>
    {#if loggedIn}

        <section class="card user-info-section">
            <h2>User Information</h2>
            <UserInfo {user}  />
        </section>

        <!-- Date picker to select the date for the journal -->
        <section class="card date-picker-section">
            <h2>Select Date</h2>
            <div class="date-picker">
                <label for="journal-date">Select a date:</label>
                <input type="date" id="journal-date" value={formatDateForInput(selectedDate)} on:change={onDateChange}>

            <!-- Button to fetch data for the selected date -->
            <button on:click={handleJournalFetch} class="fetch-button">Fetch Journal Data</button>
            </div>
        </section>

        <!-- Goal setting form -->
        <section class="card goal-section">
            <h2>Set Your Goals</h2>
            <form>
                <label for="skillPracticeGoal">{skill} Practice Goal (Hours):</label>
                <input type="number" id="skillPracticeGoal" bind:value={skillPracticeGoal} min="1">

                <label for="yogaGoal">Yoga Duration Goal (minutes):</label>
                <input type="number" id="yogaGoal" bind:value={yogaGoal} min="1">

                <label for="waterGoal">Water Intake Goal (liters):</label>
                <input type="number" id="waterGoal" bind:value={waterGoal} step="0.1" min="0.1">
            </form>
        </section>

        <!-- Main page with all journal components -->
        

        <section class="card feelings-section">
            <h2>Feelings</h2>
            <Feelings {feelings} />
        </section>

        <section class="card image-upload-section">
            <h2>Upload Image // Highlight of the Day</h2>
            <ImageUpload bind:image />
        </section>

        <section class="card reflection-section">
            <h2>Daily Reflection</h2>
            <Reflection bind:dailyReflection />
        </section>

        <section class="card skill-practice-section">
            <h2>{skill} Practice</h2>
            <SkillPractice bind:skillPractice {skill} />
        </section>

        <section class="card yoga-section">
            <h2>Yoga Duration</h2>
            <YogaDuration bind:didYoga bind:yogaDuration />
        </section>

        <section class="card water-intake-section">
            <h2>Water Intake</h2>
            <WaterIntake bind:waterIntake />
        </section>

        <!-- Submit button to save journal data -->
        <button on:click={saveJournalData} class='submit'>Submit</button>

        <!-- Productivity Pie Chart -->
        <section class="card productivity-chart-section">
            <h2>Productivity Breakdown</h2>
            <ProductivityChart {productivityData} />
        </section>

        <!-- Progress tracking for habits -->
        <section class="card progress-tracking-section">
            <h2>Progress Tracking</h2>
            <Progress habitProgress={skillPracticeProgress} habitName="Skill Practice" />
            <Progress habitProgress={yogaProgress} habitName="Yoga Duration" />
            <Progress habitProgress={waterIntakeProgress} habitName="Water Intake" />
        </section>
    {/if}

    {#if !loggedIn}
        <!-- Show login if user is not logged in -->
        <Login on:login={onLoginSuccess} />
    {/if}
</main>



