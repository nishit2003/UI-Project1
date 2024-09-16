<script>
    import Login from './components/Login.svelte';
    import Feelings from './components/Feelings.svelte';
    import ImageUpload from './components/ImageUpload.svelte';
    import Reflection from './components/Reflection.svelte';
    import SkillPractice from './components/SkillPractice.svelte';
    import UserInfo from './components/UserInfo.svelte';
    import WaterIntake from './components/WaterIntake.svelte';
    import YogaDuration from './components/YogaDuration.svelte';

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

    // Helper function to format date as YYYY-MM-DD for input value
    const formatDateForInput = (date) => {
        return date.toISOString().split('T')[0];
    };

    // Handle login success and retrieve stored data
    const onLoginSuccess = (username) => {
        user.username = username;  // Assign username after login
        loggedIn = true;
        retrieveJournalData();  // Retrieve previously saved journal data for the selected date
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
        } else {
            // If no data for the selected date, reset the fields
            feelings.forEach(feeling => feeling.checked = false);
            image = '';
            dailyReflection = '';
            skillPractice = 0;
            didYoga = false;
            yogaDuration = 0;
            waterIntake = 0;
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
        <ImageUpload bind:image  />
        <Reflection bind:dailyReflection />
        <SkillPractice bind:skillPractice />
        <YogaDuration bind:didYoga bind:yogaDuration />
        <WaterIntake bind:waterIntake />

        <!-- Submit button to save journal data -->
        <button on:click={saveJournalData}>Submit</button>
    {/if}

    {#if !loggedIn}
        <!-- Show login if user is not logged in -->
        <Login on:login={onLoginSuccess} />
    {/if}
</main>

<style>
    main {
        font-family: Arial, sans-serif;
        margin: 2rem;
    }

    button {
        padding: 0.75rem 1.5rem;
        font-size: 1rem;
        color: white;
        background-color: #007bff;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        margin-top: 1rem;
    }

    .date-picker {
        margin-bottom: 1.5rem;
    }

    .date-picker input {
        padding: 0.5rem;
        border-radius: 4px;
        border: 1px solid #ccc;
    }
</style>
