<script lang="ts">
    export let deck;
    export let RemoveDeck;
    export let AddCard;
    export let RemoveCard;
    let cardInput = "";

</script>
<div class="deck">
    <div class="button-row">
        <button class="remove-button" on:click={RemoveDeck(deck)}>X</button>
    </div>
    <div class="deck-title">{deck.title}</div>
    <div class="cards">
        <div class="card">
            <div>New card:</div>
            <textarea bind:value={cardInput} class="card-input"></textarea>
            <button on:click={() => AddCard(cardInput, deck)}>Add card</button>
        </div>
        {#each deck.cards as card}
            <div class="card">
                <div class="button-row">
                    <button class="remove-button" on:click={RemoveCard(card, deck)}>X</button>
                </div>
                <div class="card-title">{card}</div>
            </div>
        {/each}
    </div>
</div>
<style>

    .deck {
        border: 1px solid black;
        margin: 8px;
    }
    .cards {
        display: flex;
        flex-direction: row;
        overflow-x: auto;
    }
    .card {
        margin: 8px;
        display: flex;
        flex-direction: column;
        border: solid black 1px;
        height: 360px;
        min-width: 270px;
        max-width: 270px;
        align-items: center;
        justify-content: center;
        position: relative;
    }
    .button-row {
        right: 0;
        top: 0;
        display: flex;
        flex-direction: row;
        justify-content: flex-end;
        width: 100%;
    }
    .deck-title {
        text-align: center;
    }
    .remove-button {
        cursor: pointer;
    }
    .card-input {
        height: 80%;
        width: 90%;
        margin: 8px;
    }
</style>
