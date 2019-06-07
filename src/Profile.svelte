<script>
    export let displayName;
    export let photoURL;
    export let uid;
</script>

<style>
    img, .dummy {
        width: 40px;
        height: 40px;
        border-radius: 50%;
    }
    .dummy {
        background-color: #eee;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #000;
        font-weight: 600;
    }
</style>

<!-- <h3>Hi { displayName }!</h3> -->

{#if photoURL}
    <img src={ photoURL } alt="user avatar">
{:else}
    <div class="dummy">?</div>
{/if}
<!-- <p>Your userID is { uid }</p> -->