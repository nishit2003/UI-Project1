<script>
    export let user;
    export let currentDate;
    export let daysActive;
</script>

<section class="user-info">
    <!-- Use user.username or whichever property stores the username -->
    <h1>Hello, {user.username}</h1>
    <p class="date">Date: {currentDate.toDateString()}</p>
    <p> User started on {user.startDate.toDateString()}</p>
    <p class="active-days">Days Active: {daysActive} </p>
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
