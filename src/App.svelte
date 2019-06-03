<script>
  import { onMount } from "svelte";
  import { getContext } from "svelte";
  import { db } from "./firebase";
  import { collectionData } from "rxfire/firestore";
  import { startWith } from "rxjs/operators";

  import { user, eatsStore } from "./stores.js";
  import Layout from "./Layout.svelte";
  import Randomizer from "./randomizer.svelte";
  import Login from "./Login.svelte";
  import Editor from "./Editor.svelte";

  let myuser = {};
  $: eats = [];
  $: loading = true;

  // let mode = Editor; // or Randomizer
  let mode = Randomizer; // or Randomizer

  user.subscribe(value => {
    myuser = value;
  });

  $: if (myuser && myuser.uid) {
    const query = db
      .collection("eats")
      .where("uid", "==", myuser.uid)
      .orderBy("created");

    collectionData(query, "id")
      .pipe(startWith([]))
      .subscribe(_eats => {
        eatsStore.set(_eats);
        eats = _eats;
        loading = false;
      });
  }

  const switchMode = () => {
    mode = (mode === Randomizer) ? Editor : Randomizer;
  }
</script>

<style>
  .toggle-mode-button {
    position: absolute;
    top: 8rem;
    right: 0;
    font-size: 1.8rem;
  }
</style>

<svelte:head>
  <title>What are we going to eat today?</title>
</svelte:head>

<Layout>

  

  <Login />
  
  {#if !loading}

    <button class="toggle-mode-button" on:click={switchMode}>
      {#if mode == Randomizer}Edit Eats{:else}Back{/if}
    </button>
    <svelte:component this={mode} foodList={eats}/>
  {/if}

  <!-- <Editor uid={myuser.uid} /> -->

  <!-- <Randomizer foodList={eats}/> -->

</Layout>
