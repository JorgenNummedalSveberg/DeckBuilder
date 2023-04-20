<script lang="ts">
  import Deck from "./Deck.svelte";
  import jsPDF from 'jspdf';

  let decks: Deck[] = [];
  let titleInput = "";
  let fileName = "";
  let includeBacks = false;
  let fileInput;

  function AddCard(name: string, deck: DeckInterface) {
    if (name === "") return;
    deck.cards = [...deck.cards, name];
    decks[decks.indexOf(deck)] = deck;
  }

  function AddDeck(title: string) {
    if (title === "") return;
    decks = [...decks, {cards: [], title: title}]
  }
  function RemoveDeck(deck: DeckInterface) {
    decks = decks.filter(x => x !== deck)
  }

  type DeckInterface = {
    title: string;
    cards: string[];
  }

  function ExportJson(name: string) {
    if (name === "") return;
    if (decks.length === 0) return;
    decks.forEach((x: Deck) => {
      if (x.cards.length === 0) return;
    })
    const dataStr = JSON.stringify(decks);
    const dataUri = 'data:application/json;charset=utf-8,' + encodeURIComponent(dataStr);
    const downloadLink = document.createElement('a');
    downloadLink.href = dataUri;
    downloadLink.download = fileName+'.json';
    document.body.appendChild(downloadLink);
    downloadLink.click();
    document.body.removeChild(downloadLink);
  }

  function ImportJson() {
    const reader = new FileReader();
    reader.onload = () => {
      try {
        decks = JSON.parse(reader.result)
      } catch (error) {
        console.error(error);
      }
    };
    reader.readAsText(fileInput.files[0]);
  }
  function ExportPDF(name: string) {
    if (name === "") return;
    if (decks.length === 0) return;
    decks.forEach((x: Deck) => {
      if (x.cards.length === 0) return;
    })

    const doc = new jsPDF({format: [300, 400]});

    for (let i = 0; i < decks.length; i++){
      let deck = decks[i];
      // Add a new page
      if (i != 0) doc.addPage();

      let x = 0;
      let y = 0;
      let backLength = 0;
      for (let j = 0; j < deck.cards.length; j++) {
        const cell = {
          x: x*75,
          y: y*100,
          w: 75,
          h: 100,
          text: deck.cards[j],
          align: 'center', // Center the text within the cell
          valign: 'middle', // Center the cell vertically within the row
          lineWidth: 0.1,
        };
        doc.rect(cell.x, cell.y, cell.w, cell.h);
        doc.text(cell.x + cell.w / 2, cell.y + cell.h / 2, cell.text, null, null, 'center');
        backLength++;
        x++;
        if (x > 3) {
          x = 0;
          y ++;
          if (y > 3) {
            y = 0;
            doc.addPage();
            if (includeBacks) {
              AddBacks(doc, deck.title, backLength)
              if (j != deck.cards.length-1) {
                doc.addPage()
              }
              backLength = 0;
            }
          }
        }
      }
      if (includeBacks) {
        doc.addPage();
        AddBacks(doc, deck.title, backLength)
        backLength = 0;
      }
    }

    doc.save(name + '.pdf');
  }

  function AddBacks(doc, title: string, length: number) {
    let x = 3;
    let y = 0;
    for (let i = 0; i < length; i++) {
      const cell = {
        x: x*75,
        y: y*100,
        w: 75,
        h: 100,
        text: title,
        align: 'center', // Center the text within the cell
        valign: 'middle', // Center the cell vertically within the row
        lineWidth: 0.1,
      };
      doc.rect(cell.x, cell.y, cell.w, cell.h);
      doc.text(cell.x + cell.w / 2, cell.y + cell.h / 2, cell.text, null, null, 'center');

      x--;
      if (x < 0) {
        x = 3;
        y ++;
      }
    }
  }

</script>
<main>
  <div class="deckInput">
    <input bind:value={titleInput}>
    <button on:click={() => AddDeck(titleInput)}>Add deck</button>
    <button on:click={() => decks = []}>Remove decks</button>
    <div></div>
    <input bind:value={fileName}>
    <div>
      Include backs
      <input bind:value={includeBacks} type="checkbox">
    </div>
    <button on:click={() => ExportJson(fileName)}>Export deck as JSON</button>
    <button on:click={() => ExportPDF(fileName)}>Export deck as PDF</button>
    <input bind:this={fileInput} type="file">
    <button on:click={() => ImportJson()}>Import deck from JSON</button>
  </div>
  <div class="decks">
    {#each decks as deck}
      <Deck deck={deck} AddCard={AddCard} RemoveDeck={RemoveDeck}/>
    {/each}
  </div>
</main>
<style>
  main {
    display: flex;
    flex-direction: column;
  }
  .deckInput {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    padding: 8px;
    align-items: center;
  }
  .decks{
    display: flex;
    flex-direction: column;
    padding: 8px;
  }
</style>
