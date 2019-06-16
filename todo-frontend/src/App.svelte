<script>
	import Whiteboard from "./components/Whiteboard.svelte";
	import axios from "axios";
    axios.defaults.baseURL = 'http://localhost:5000';

	let response = init(); 
	let goToRegister = false;
	let goToLogin = false;
	let registerMsg = "";
	let userId;

	const createPoint = "/user/register";
	const authPoint = "/user/authenticate";

	async function init() { return null}

	async function requestToken() {
		const user = validateForm("login");
		try {
			
			const r = await axios.post(authPoint, user)
			main();
			console.log(r);
			
			return r
		} catch (error) {
			login();
			throw new Error("Could not authenticate user")
		}
	}

	async function registerUser() {
		const user = validateForm("register", e);
		console.log(user);
		
		try {
			await axios.post(createPoint, user);
			console.log("worked");
			
			alert("Account created!")
			main();
		} catch (error) {
			console.log(error);
			register()
			registerMsg = "Account already exists!"
		}
	}

	function validateForm(formname) {
		const user = {}
		if (formname == "register")
			user.Email = document.forms[formname]["email"].value;
		user.Username = document.forms[formname]["username"].value;
		user.Password = document.forms[formname]["password"].value;
		return user;
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
	<Whiteboard username={"ttt"} id={1} token={"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6IjEiLCJuYmYiOjE1NjA0NTU3MzQsImV4cCI6MTU2MTA2MDUzNCwiaWF0IjoxNTYwNDU1NzM0fQ._pOkcKstj1JA67wFr-D6gN7Q28ghr8AyrNHPt9SJM2s"}/>

{#await response then resp}
	{#if resp != null}
		<Whiteboard {...resp.data}/>
	{:else}
		{#if goToRegister}
		<p class="register-msg">{registerMsg}</p>
		<form name="register" on:submit|preventDefault={registerUser}>
			<label for="email">Email</label>
			<input type="text" placeholder="Enter Email" name="email" required>

			<label for="username">Username</label>
			<input type="text" placeholder="Enter Username" name="username" required>

			<label for="password">Password</label>
			<input type="password" placeholder="Enter Password" name="password" required>			
			
			<br>
			<button type="submit">Submit</button>
			<br>
			<p>Already have an account? <button on:click={login}>Sign in</button></p> 
			<br>
			<button on:click={main}>Go back</button>
		</form>
	{:else if goToLogin}
		<p class="register-msg">{registerMsg}</p>
		<form name="login" on:submit|preventDefault="{()=> response = requestToken()}">
			<label for="username">Username</label>
			<input type="text" placeholder="Enter Username" name="username" required>

			<label for="password">Password</label>
			<input type="password" placeholder="Enter Password" name="password" required>			
			
			<br>
			<button type="submit">Login</button>
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
{:catch err}
	<p>{err}</p>
{/await}


