<script>
  import {form} from 'svelte-forms'
  import { postData } from './_helpers/apiCall.js'

  let firstname = "", lastname = "", email = "", password = ""
  let firstnameTouched = false, lastnameTouched = false, emailTouched = false, passwordTouched = false

  const loginForm = form(() => ({
    firstname: { value: firstname, validators: ["required"] },
    lastname: { value: lastname, validators: ["required"] },
    email: { value: email, validators: ["required", "email"] },
    password: { value: password, validators: ["required", "min:6"] },
  }))

  const touchFirstName = () => {
    firstnameTouched = true
  }

  const touchLastname = () => {
    lastnameTouched = true
  }

  const touchEmail = () => {
    emailTouched = true
  }

  const touchPassword = () => {
    passwordTouched = true
  }

  const signup = async () => {
    const user = {
      firstname: firstname,
      lastname: lastname,
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

<h4>SIGN UP</h4>

{#if $loginForm.firstname.errors.includes('required') && firstnameTouched }
  <p>Fisrtname cannot be empty</p>
{/if}

{#if $loginForm.lastname.errors.includes('required') && lastnameTouched }
  <p>Lastname cannot be empty</p>
{/if}

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
  <label for="signup-firstname">Firstname</label>
  <input type="text" id="signup-firstname" bind:value={firstname} on:change={touchFirstName} />

  <label for="signup-lastname">Lastname</label>
  <input type="text" id="signup-lastname" bind:value={lastname} on:change={touchLastname} />

  <label for="signup-email">Email</label>
  <input type="text" id="signup-email" bind:value={email} on:change={touchEmail} />

  <label for="signup-password">Password</label>
  <input type="password" id="signup-password" bind:value={password} on:change={touchPassword}/>

  <button type="button" disabled={!$loginForm.valid} on:click={signup}>Login</button>
</form>

<a href="/login">GOT AN ACCOUNT? LOG IN</a>