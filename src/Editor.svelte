<script>
    // import TodoItem from './TodoItem.svelte';
    import EatItem from './EatItem.svelte'
    import { db } from './firebase';
    import { collectionData } from 'rxfire/firestore';
    import { startWith } from 'rxjs/operators';
    import {eatsStore, user} from './stores.js'

    let newEatText = '';
    let newEatDistance = null;
    let newEatPrice = null;
    let uid = null;

    user.subscribe(value => {
        uid = value.uid;
    })

    // eatsStore.subscribe(value => {
    //     console.log({value});
    //     eats = value;
    // });

    function add() {
        db.collection('eats').add({ uid, name: newEatText, price: newEatPrice, distance: newEatDistance, created: Date.now() });
        newEatText = '';
        newEatDistance = null;
        newEatPrice = null;
    }

    // function updateStatus(event) {
    //     console.log(event);
    //     const { id, newStatus } = event.detail;
    //     // db.collection('eats').doc(id).update({ complete: newStatus });
    // }

    function removeItem(event) {
        const { id } = event.detail.eat;
        db.collection('eats').doc(id).delete();
    }
</script>

<style>
    input { display: block }

    form {
        width: 100%;
        padding: 0 1rem;
    }
    
    .form-row {
        display: flex;
        flex-direction: row;
        align-items: center;
    }

    ul.existing-eats-list {
        margin: 0;
        padding: 0;
        list-style: none;
        width: 100%;
        padding: 0 1rem;
    }

    hr {
        width: 50px;
        margin-top: 1rem;
        margin-bottom: 1rem;
    }

    hr + h2 {
        margin-top: 0.5rem;
    }
</style>



<form on:submit|preventDefault={add} class="add-form">
    <div class="form-row">
        <div class="form-element">
            Name:
            <input type='text' bind:value={newEatText}>
        </div>


        <div class="form-element">
            Distance:
            <select bind:value={newEatDistance}>
                <option value=''>n/a</option>
                <option value='near'>Near</option>
                <option value='medium'>Medium</option>
                <option value='far'>Far</option>
            </select>
        </div>

        <div class="form-element">
            Price:
            <select bind:value={newEatPrice}>
                <option value=''>n/a</option>
                <option value='cheap'>Cheap</option>
                <option value='pricey'>Pricey</option>
            </select>
        </div>
    </div>

    <button type='submit' on:click={add}>➕ Add Eat</button>
</form>

<hr/>

<h2>Existing Eats:</h2>
<ul class="existing-eats-list">

	{#each $eatsStore as eat}
    
        <EatItem {eat} on:remove={removeItem} />
        
	{/each}
</ul>
