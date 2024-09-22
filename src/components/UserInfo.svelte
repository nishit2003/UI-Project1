<script>
    export let user; 
    let currentDate = new Date(); // Current date for user information
    let startDate = new Date('2024-09-11'); // User's start date as a Date object
    let daysActive = Math.floor((currentDate.getTime() - startDate.getTime()) / (1000 * 60 * 60 * 24)); // Difference in days
</script>

<section class="user-info">
    <!-- Use user.username or whichever property stores the username -->
    <h1>Hello, {user.Name}</h1>
    <p class="date">Date: {currentDate.toDateString()}</p>
    <p>User started on Sep 11, 2024</p>
    <p class="active-days">Days Active: {daysActive}</p>
</section>


<style>
    .user-info {
        text-align: center;
        background-color: #f4f4f4; /* Light background to make the text stand out */
        padding: 20px;
        border-radius: 10px; /* Soft rounded corners for a modern look */
        max-width: 400px;
        margin: 0 auto; /* Center the section */
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
        font-family: 'Arial', sans-serif; /* Clean and readable font */
    }

    .user-info h1 {
        font-size: 2rem;
        margin-bottom: 10px;
        color: #333; /* Darker text for good contrast */
    }

    .user-info p {
        margin: 0.5rem 0;
        color: #555; /* Softer text color for less emphasis */
    }

    .user-info .date, .user-info .active-days {
        font-size: 1.2rem; /* Slightly increased font size for readability */
        font-weight: 500;
        color: #333;
    }

    .user-info .active-days {
        background-color: #e0f7fa; /* Highlight active days with a soft background color */
        padding: 10px;
        border-radius: 5px;
        display: inline-block; /* Keep the highlight contained */
    }

    /* Add hover effect to improve interaction */
    .user-info h1:hover {
        color: #00796b; /* Change color on hover for interactivity */
        transition: color 0.3s ease;
    }

    .user-info .active-days:hover {
        background-color: #b2ebf2; /* Slight change on hover for active days */
        transition: background-color 0.3s ease;
    }
</style>
