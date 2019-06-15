<script>
	import Whiteboard from "./components/Whiteboard.svelte";
	import axios from "axios";

	let token; 
	let goToRegister = false;
	let goToLogin = false;
	let registerMsg = "";

	const user = {
		email: "tae1s1a1t@hotmail.com", 
		username: "tesa21a1ter", 
		password: "password1"
	}
	const createPoint = "http://localhost:5000/user/register";
	const authPoint = "http://localhost:5000/user/authenticate";

	async function getToken() {
		try {
			token = axios.post(authPoint, user)
		} catch (error) {
			throw new Error("Could not authenticate user")
		}
	}

	async function validateForm(e) {
		e.preventDefault();
		const Email = document.forms["register"]["email"].value;
		const Username = document.forms["register"]["username"].value;
		const Password = document.forms["register"]["password"].value;
		const user1 = {
			Email,
			Username,
			Password
		}
		try {
			await axios.post(createPoint, user1);
			alert("Account created!")
			goToRegister = false;
		} catch (error) {
			registerMsg = "Account already exists!"
			
			goToRegister = true
		}
	}

	function loginForm(e) {
		e.preventDefault();

	}

	function register() {
		goToLogin = false;
		goToRegister = true;
		registerMsg = "Register a new user"
	}

	function login() {
		goToRegister = false;
		goToLogin = true;
		registerMsg = "Login"
	}

	function main() {
		goToLogin = false;
		goToRegister = false;
		registerMsg = ""
	}
</script>
<style>
.register-msg{
	color: red;
}
</style>

{#if token}
	<Whiteboard token={token}/>
{:else}
	{#if goToRegister}
		<p class="register-msg">{registerMsg}</p>
		<form name="register">
			<label for="email">Email</label>
			<input type="text" placeholder="Enter Email" name="email" required>

			<label for="username">Username</label>
			<input type="text" placeholder="Enter Username" name="username" required>

			<label for="password">Password</label>
			<input type="text" placeholder="Enter Password" name="password" required>			
			
			<br>
			<button on:click={validateForm}>Submit</button>
			<br>
			<p>Already have an account? <button on:click={login}>Sign in</button></p> 
			<br>
			<button on:click={main}>Go back</button>
		</form>
	{:else if goToLogin}
		<p class="register-msg">{registerMsg}</p>
		<form name="login">
			<label for="email">Email</label>
			<input type="text" placeholder="Enter Email" name="email" required>

			<label for="username">Username</label>
			<input type="text" placeholder="Enter Username" name="username" required>

			<label for="password">Password</label>
			<input type="text" placeholder="Enter Password" name="password" required>			
			
			<br>
			<button on:click={loginForm}>Submit</button>
			<br>
			<p><button on:click={register}>Register</button></p> 
			<br>
			<button on:click={main}>Go back</button>
		</form>
	{:else}
		<button on:click={login}>Login</button>
		<button on:click={register}>Register</button>
	{/if}
{/if}

