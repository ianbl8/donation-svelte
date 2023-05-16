<script>
  import Header from '$lib/Header.svelte';
  import WelcomeNavigator from '$lib/WelcomeNavigator.svelte';
  import LoginForm from '$lib/LoginForm.svelte';
</script>

<Header>
  <WelcomeNavigator />
</Header>

<div class="columns">
  <div class="column has-text-centered">
      <img alt="homer" src="/homer2.png" width="300" />
  </div>
  <div class="column">
      <div class="box">
          <h1 class="title">Log in</h1>
          <LoginForm />
      </div>
  </div>
</div>