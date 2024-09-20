<script>
    import { createEventDispatcher } from 'svelte';
    
    let username = '';
    let password = '';
    let isLogin = true;  // Toggle between login and sign-up
    const dispatch = createEventDispatcher(); // Create dispatcher to send login event

    // Function to handle login
    const handleLogin = () => {
        const storedUser = JSON.parse(localStorage.getItem(username));
        
        if (storedUser && storedUser.password === password) {
            alert(`Welcome back, ${username}!`);
            dispatch('login', username);  // Emit login event with username
        } else {
            alert('Invalid username or password.');
        }
    };

    // Function to handle sign-up
    const handleSignUp = () => {
        const storedUser = JSON.parse(localStorage.getItem(username));

        if (storedUser) {
            alert('User already exists. Please login.');
        } else {
            const newUser = { username, password };
            localStorage.setItem(username, JSON.stringify(newUser));
            alert('Account created! Please login.');
            isLogin = true;  // Switch back to login mode
        }
    };

    // Function to handle form submission based on mode
    const handleSubmit = () => {
        if (isLogin) {
            handleLogin();
        } else {
            handleSignUp();
        }
    };

    // Toggle between Login and Sign-Up
    const toggleMode = () => {
        isLogin = !isLogin;
    };
</script>

<style>
    .login-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background-color: #f0f4f8;
    }
    .header {
        font-size: 2rem;
        color: #007bff;
        margin-bottom: 1rem;
        text-align: center;
    }
    .login-box {
        padding: 2rem;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        background-color: white;
    }
    input {
        margin: 0.5rem 0;
        padding: 0.5rem;
        width: 100%;
        border-radius: 4px;
        border: 1px solid #ccc;
    }
    button {
        margin-top: 1rem;
        padding: 0.5rem 1rem;
        border: none;
        border-radius: 4px;
        background-color: #007bff;
        color: white;
        cursor: pointer;
    }
    p {
        margin-top: 1rem;
        cursor: pointer;
        color: #007bff;
    }
</style>

<div class="login-container">
    <div class="header">Welcome to Health Tracker</div>
    <div class="login-box">
        <h2>{isLogin ? 'Login' : 'Sign Up'}</h2>
        <input type="text" bind:value={username} placeholder="Username" />
        <input type="password" bind:value={password} placeholder="Password" />
        <button on:click={handleSubmit}>{isLogin ? 'Login' : 'Sign Up'}</button>
        <button on:click={toggleMode}>
            {isLogin ? "Don't have an account? Sign up" : 'Already have an account? Login'}
        </button>
    </div>
</div>
