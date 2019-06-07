<script>
  import Profile from "./Profile.svelte";

  import { user } from "./stores.js";

  import { auth, googleProvider } from "./firebase";
  import { authState } from "rxfire/auth";

  let myuser;
  $: loading = true;

  const unsubscribe = authState(auth).subscribe(u => {
    myuser = u;
    user.set(u);
    loading = false;
  });

  function login() {
    auth.signInWithPopup(googleProvider);
  }

  function loginAnonym() {
    auth.signInAnonymously();
  }
</script>


<style>
  section {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  p {
    margin: 1rem 0;
    font-size: 1.5rem;
  }
  p.tagline {
    margin-bottom: 3rem;
  }
  p.to-add {
    margin-top: 1.2rem;
    margin-bottom: 3rem;
  }
  button {
    font-size: 1.8rem;
    margin-bottom: 0;
  }
  .orrrr {
    margin-top: 0.3rem;
    margin-bottom: 0.3rem;
  }
</style>


<section>
  {#if !myuser && !loading}
    <p class="tagline">Can't decide what to eat?</p>

    <button on:click={login}>Login with Google</button>
    <i class="orrrr">or</i>
    <button on:click={loginAnonym}>Login anonymously</button>
    <p class="to-add">to add some eats</p>
  {/if}
</section>
