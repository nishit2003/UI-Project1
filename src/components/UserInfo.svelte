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
    }

    .user-info .date, .user-info .active-days {
        font-size: 1.1rem;
        margin-top: 0.5rem;
    }
</style>
