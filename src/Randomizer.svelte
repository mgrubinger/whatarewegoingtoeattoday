<script>
import { random } from 'lodash'
import { fly } from 'svelte/transition'
import { cubicOut } from 'svelte/easing';
import Emojis from './emojis.js'

import { createEventDispatcher } from 'svelte'

export let foodList;

let foodEmojiList = Emojis;
$: currentFoodEmoji = " ";

let disabled = false;
$: chosenFood = null;

const dispatch = createEventDispatcher()

function rand(timeout) {

    // stop emojis and show random food
    if(timeout > 300) {
        disabled = false;
        getRandomFood()
        currentFoodEmoji = "";
        return;
    }
    
    // call the next iteration
    window.setTimeout(() => {
        currentFoodEmoji = foodEmojiList[random(0, foodEmojiList.length-1)];
        timeout *= 1.08;
        rand(timeout)
    }, timeout);
}

// kick it off
let go = () => {
    disabled = true;
    chosenFood = null;
    rand(10);
} 

let getRandomFood = () => {
    chosenFood = foodList[random(0, foodList.length-1)];
    chosenFood = {...chosenFood};
    chosenFood.metastring = stringifyMeta(chosenFood);
}

let stringifyMeta = ({price, distance}) => {
    if(price == "cheap" && distance == "near") return "Cheap and closeby!"
    if(price == "cheap" && distance == "medium") return "Cheap and not too far away!"
    if(price == "cheap" && distance == "far") return "Cheap, but quite the walk!"
    if(price == "pricey" && distance == "near") return "Near, but quite pricey!"
    if(price == "pricey" && distance == "medium") return "Not too far, but quite pricey!"
    if(price == "pricey" && distance == "far") return "Pricey and far away..."
    if(price == null && distance == null) return ``
    if(price != null && distance == null) return `Price: ${price}`
    if(price == null && distance != null) return `Distance: ${distance}`
}

let goToEdit = () => {
    dispatch('goToEdit')
}

</script>

<style>
.randomfoodemoji {
    font-size: 8rem;
}
.randomizer-button {
    width: 40vw;
    height: 40vw;
    border-radius: 50%;
    font-size: 4rem;
    max-width: 250px;
    max-height: 250px;
}
.randomizer-button[disabled] {
    display: none;
}

.chosen-food {
    font-size: 1.8rem;
}

.chosen-food h1 {
    font-size: 3rem;
    margin-bottom: 1rem;
}
.chosen-food p {
    margin-top: 0.5em;
}
</style>

<section>

    <div class="randomfoodemoji">
        {currentFoodEmoji}
    </div>
    
    {#if chosenFood}
        <div  class="chosen-food" in:fly="{{ y: 200, duration: 2000, opacity: 0, easing: cubicOut }}">
            <h1>{chosenFood.name}</h1>
            <p>
                {chosenFood.metastring}
            </p>
        </div>
    {/if}

    {#if foodList.length > 0}
        <button class="randomizer-button" {disabled} on:click={go}>Shuffle!</button>
    {:else}
        Start by <button on:click={goToEdit}>adding some eats</button>
    {/if}
</section>