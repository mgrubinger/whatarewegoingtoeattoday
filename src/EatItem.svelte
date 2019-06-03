<script>
  import { createEventDispatcher } from 'svelte'
  import { fade } from 'svelte/transition'
  import { debounce } from 'lodash'

  const dispatch = createEventDispatcher()

  function remove() {
    dispatch('remove', {eat})
  }

  function updateText() {
    dispatch('update', {eat})
  }
  // We don’t want to clobber the local DB, so we debounce saving on every keystroke
  const debouncedUpdateText = debounce(updateText, 500)

  export let name
  export let distance
  export let price

  export let eat;

</script>

<style>
  button {
    border-radius: 50%;
    width: 2.25em;
    height: 2.25em;
    line-height: 1.3em;
    text-align: center;
    margin-left: 0.75em;
    padding: 0;
  }

  li {
    width: 100%;
    display: flex;
    align-items: center;
  }

  li button {
    background-color: transparent;
    border: none;
  }
  li p {
    width: 100%;
    font-size: 1.3rem;
  }
</style>

<li>
  <p>
    <strong>{eat.name}</strong>
    {#if eat.distance}<br/><small>Distance: {eat.distance}</small>{/if}
    {#if eat.price}<br/><small>Price: {eat.price}</small>{/if}
  </p>
  <button on:click={remove}>🗑</button>
</li>
