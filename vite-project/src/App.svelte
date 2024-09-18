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

    let loggedIn = false;  // Track if user is logged in
    let user = { username: '', startDate: new Date() };
    let currentDate = new Date();  // Current date for user information
    let selectedDate = new Date();  // Default is today's date
    let daysActive = 15;

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

    // Productivity data for chart
    let productivityData = [skillPractice, yogaDuration, waterIntake];

    // Helper function to format date as YYYY-MM-DD for input value
    const formatDateForInput = (date) => {
        return date.toISOString().split('T')[0];
    };

    // Handle login success and retrieve stored data
    const onLoginSuccess = (username) => {
        user.username = username;  // Assign username after login
        loggedIn = true;
        retrieveJournalData();  
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
        productivityData = [skillPractice, yogaDuration, waterIntake];

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
            productivityData = [skillPractice, yogaDuration, waterIntake];
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
        }
    };
</script>

<main>
    {#if loggedIn}
        <!-- Date picker to select the date for the journal -->
        <div class="date-picker">
            <label for="journal-date">Select a date:</label>
            <input type="date" id="journal-date" value={formatDateForInput(selectedDate)} on:change={onDateChange}>
        </div>

        <!-- Main page with all journal components -->
        <UserInfo {user} {currentDate} {daysActive} />
        <Feelings {feelings} />
        <ImageUpload bind:image />
        <Reflection bind:dailyReflection />
        <SkillPractice bind:skillPractice />
        <YogaDuration bind:didYoga bind:yogaDuration />
        <WaterIntake bind:waterIntake />

        <!-- Submit button to save journal data -->
        <button on:click={saveJournalData} class='submit'>Submit</button>

        <!-- Productivity Pie Chart -->
        <section>
            <h2>Productivity Breakdown</h2>
            <ProductivityChart {productivityData} />
        </section>
    {/if}

    {#if !loggedIn}
        <!-- Show login if user is not logged in -->
        <Login on:login={onLoginSuccess} />
    {/if}
</main>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

    :root {
        --primary-color: #6110ee;
        --primary-color-dark: #3700b3;
        --accent-color: #03dac6;
        --text-color: #333;
        --background-color: #f0f4f8;
        --error-color: #b00020;
        --font-family: 'Roboto', sans-serif;
    }

    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: var(--font-family);
        color: var(--text-color);
        background-color: var(--background-color);
        line-height: 1.6;
    }

    main {
        max-width: 900px;
        margin: 2rem auto;
        padding: 2rem;
        background-color: #fff;
        border-radius: 16px;
        box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
    }

    button {
        background: linear-gradient(45deg, var(--primary-color), var(--primary-color-dark));
        color: white;
        padding: 0.75rem 1.5rem;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        font-size: 1rem;
        transition: background 0.3s, transform 0.2s;
    }

    button:hover {
        background: linear-gradient(45deg, var(--primary-color-dark), var(--primary-color));
        transform: translateY(-2px);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    button:active {
        transform: translateY(0);
        box-shadow: none;
    }

    .date-picker {
        display: flex;
        align-items: center;
        margin-bottom: 1rem;
    }

    .date-picker label {
        font-size: 1rem;
        margin-right: 0.5rem;
        color: var(--text-color);
    }

    .date-picker input[type="date"] {
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 8px;
    }

    .submit {
        background: linear-gradient(45deg, var(--primary-color), var(--primary-color-dark));
        color: white;
        padding: 0.75rem 1.5rem;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        font-size: 1rem;
        transition: background 0.3s, transform 0.2s;
    }

    section h2 {
        margin-top: 2rem;
        color: var(--primary-color);
    }
</style>
