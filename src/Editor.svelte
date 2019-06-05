<script>
  import { eatsStore, user } from "./stores.js";
  import EatItem from "./EatItem.svelte";

  let newEatText = "";
  let newEatDistance = null;
  let newEatPrice = null;
  let uid = null;

  user.subscribe(value => {
    if(value.uid)
      uid = value.uid;
  });

  function add() {
    db.collection("eats").add({
      uid,
      name: newEatText,
      price: newEatPrice,
      distance: newEatDistance,
      created: Date.now()
    });
    newEatText = "";
    newEatDistance = "";
    newEatPrice = "";
  }

  function removeItem(event) {
    const { id } = event.detail.eat;
    db.collection("eats")
      .doc(id)
      .delete();
  }
</script>


<style>
  input,
  select {
    display: block;
    margin-bottom: 0;
    margin-left: 0.3rem;
  }

  button[type="submit"] {
    margin-top: 1.5rem;
  }

  form {
    width: 100%;
    padding: 0 1rem;
    font-size: 1.3rem;
  }

  .form-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
  }
  .form-element {
    padding: 0 0.5rem;
    display: flex;
    align-items: center;
    margin-bottom: 0.5rem;
  }

  ul.existing-eats-list {
    margin: 0;
    padding: 0;
    list-style: none;
    width: 100%;
    max-width: 300px;
    padding: 0 1rem;
  }

  hr {
    width: 50px;
    margin-top: 1.5rem;
    margin-bottom: 1.5rem;
  }

  hr + h2 {
    margin-top: 0.5rem;
    color: var(--pink);
  }
</style>


<form on:submit|preventDefault={add} class="add-form">
  <div class="form-row">
    <div class="form-element">
      Name:
      <input type="text" bind:value={newEatText} />
    </div>

    <div class="form-element">
      Distance:
      <select bind:value={newEatDistance}>
        <option value="">n/a</option>
        <option value="near">Near</option>
        <option value="medium">Medium</option>
        <option value="far">Far</option>
      </select>
    </div>

    <div class="form-element">
      Price:
      <select bind:value={newEatPrice}>
        <option value="">n/a</option>
        <option value="cheap">Cheap</option>
        <option value="pricey">Pricey</option>
      </select>
    </div>
  </div>

  <button type="submit">➕ Add Eat</button>
</form>

<hr />

<h2>Existing Eats:</h2>

<ul class="existing-eats-list">

  {#each $eatsStore as eat}
    <EatItem {eat} on:remove={removeItem} />
  {/each}
</ul>
