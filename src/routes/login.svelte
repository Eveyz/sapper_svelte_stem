<script>
  import {form} from 'svelte-forms'
  import { postData } from './_helpers/apiCall.js'

  let email = ""
  let emailTouched = false
  let password = ""
  let passwordTouched = false

  const loginForm = form(() => ({
    email: { value: email, validators: ["required", "email"] },
    password: { value: password, validators: ["required", "min:6"] },
  }))

  const touchEmail = () => {
    emailTouched = true
  }

  const touchPassword = () => {
    passwordTouched = true
  }

  const login = async () => {
    console.log(email)
    console.log(password)
    const user = {
      email: email,
      password: password
    }
    try {
      const data = await postData('http://localhost:5000', user);
      console.log(JSON.stringify(data)); // JSON-string from `response.json()` call
    } catch (error) {
      console.error(error);
    }
  }

</script>

<h4>LOGIN</h4>

{#if $loginForm.email.errors.includes('email') }
  <p>Email should be in the right format</p>
{/if}
{#if $loginForm.email.errors.includes('required') && emailTouched }
  <p>Email cannot be empty</p>
{/if}

{#if $loginForm.password.errors.includes('min') }
  <p>Password should be at least 6 characters</p>
{/if}
{#if $loginForm.password.errors.includes('required') && passwordTouched }
  <p>Password cannot be empty</p>
{/if}

<form>
  <label for="login-email">Email</label>
  <input type="text" id="login-email" bind:value={email} on:change={touchEmail} />

  <label for="login-password">Password</label>
  <input type="password" id="login-password" bind:value={password} on:change={touchPassword}/>

  <button type="button" disabled={!$loginForm.valid} on:click={login}>Login</button>
</form>

<a href="/join">NEED AN ACCOUNT? SIGNUP</a>