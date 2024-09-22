<script>
    import { createEventDispatcher } from 'svelte';
    
    let username = '';
    let password = '';
    let Name = '';
    let isLogin = true;  // Toggle between login and sign-up
    const dispatch = createEventDispatcher(); // Create dispatcher to send login event

    // Function to handle login
    const handleLogin = () => {
        const storedUser = JSON.parse(localStorage.getItem(username));
        
        if (storedUser && storedUser.password === password) {
            alert(`Welcome back, ${Name}!`);
            dispatch('login', { username, Name });  // Emit login event with username
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
            const newUser = { username, password, Name };
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

    const quotes = [
        "Your health is your wealth.",
        "A healthy outside starts from the inside.",
        "Take care of your body, it's the only place you have to live.",
        "Health is not about the weight you lose but the life you gain."
    ];

    let randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
</script>

<style>
    .login-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background: var(--login-container-bg); /* Soft gradient background */
        font-family: 'Arial', sans-serif;
        padding: 1rem;
        transition: background 0.3s ease-in-out; /* Smooth transition when changing theme */

    }

    .header {
        font-size: 2.5rem;
        color: white;
        margin-bottom: 2rem;
        text-align: center;
        font-weight: bold;
        text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
    }

    .quote-box {
        background-color: rgba(255, 255, 255, 0.8);
        border-radius: 10px;
        padding: 1.5rem;
        font-size: 1.2rem;
        color: #333;
        margin-bottom: 1.5rem;
        text-align: center;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    }

    .login-box {
        padding: 2rem;
        border-radius: 10px;
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
        background-color: white;
        max-width: 400px;
        width: 100%;
        text-align: center;
    }

    input {
        margin: 0.75rem 0;
        padding: 0.75rem;
        width: 100%;
        border-radius: 6px;
        border: 1px solid #ddd;
        font-size: 1rem;
    }

    button {
        margin-top: 1.5rem;
        padding: 0.75rem 1rem;
        border: none;
        border-radius: 6px;
        background-color: #007bff;
        color: white;
        font-size: 1.1rem;
        cursor: pointer;
        width: 100%;
    }

    button:hover {
        background-color: #0056b3;
    }

    .toggle-button {
        margin-top: 1rem;
        background-color: transparent;
        color: #007bff;
        border: none;
        cursor: pointer;
        font-size: 0.9rem;
    }

    .toggle-button:hover {
        text-decoration: underline;
    }

    @media (max-width: 600px) {
        .login-box {
            padding: 1.5rem;
        }

        .header {
            font-size: 2rem;
        }
    }
</style>

<div class="login-container">
    <div class="header">Welcome to Your Health Tracker</div>

    <!-- Random quote for motivation -->
    <div class="quote-box">
        <p>"{randomQuote}"</p>
    </div>

    <div class="login-box">
        <h2>{isLogin ? 'Login' : 'Sign Up'}</h2>
        <input type="text" bind:value={username} placeholder="Username" />
        <input type="password" bind:value={password} placeholder="Password" />
        <input type="text" bind:value={Name} placeholder="Name" />
        <button on:click={handleSubmit}>{isLogin ? 'Login' : 'Sign Up'}</button>
        <button class="toggle-button" on:click={toggleMode}>
            {isLogin ? "Don't have an account? Sign up" : 'Already have an account? Login'}
        </button>
    </div>
</div>
