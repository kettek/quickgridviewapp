<script lang='ts'>
import { onMount } from "svelte";


  //import { promises as fsPromises } from 'fs'

  let backgroundColor: string = '#444444'
  let itemsPerRow: number = 6
  let itemCSS: string = `width: 128px;
height: 128px;
object-fit: contain;
padding: 1em;
margin: 1em;
  `

  let showGrid = false

  let files: string[] = []

  function openFolder(e: Event) {
    if (!e.target) return
    let el = <HTMLInputElement>(e.target)
    for (let file of el.files) {
      files.push(`file://${file.path}`)
    }
    files = [...files]
  }

  onMount(() => {
    window.addEventListener('keyup', (e: KeyboardEvent) => {
      if (e.key === 'Escape') {
        showGrid = false
      }
    })
  })

</script>

<main class:gridMode={showGrid}>
  {#if showGrid}
    <section style="background: {backgroundColor}; grid-template-columns: repeat({itemsPerRow}, 1fr)">
      {#each files as file}
        <img src={file} style={itemCSS}>
      {/each}
    </section>
  {:else}
    <section class='settings'>
      <label>
        Background:
        <input type='color' bind:value={backgroundColor}>
      </label>
      <label>
        Items Per Row:
        <input type='number' bind:value={itemsPerRow}>
      </label>
      <label>
        Item CSS:
        <textarea bind:value={itemCSS}></textarea>
      </label>
    </section>
    <section class='gogogo'>
      <label>
        Select a folder to display icons from.
        <input type='file' webkitdirectory on:change={openFolder}>
      </label>
      <button on:click={()=>showGrid=!showGrid}>
      Click here to show grid. Hit escape to return.
      </button>
    </section>
  {/if}
</main>

<style>
  main {
    width: 100%;
    height: 100%;
    display: grid;
    grid-template-columns: auto minmax(0, 1fr);
    background: #222222;
    color: #aaaaaa;
  }
  main.gridMode {
    grid-template-columns: minmax(0, 1fr);
    align-items: flex-start;
  }
  main.gridMode > section {
    display: grid;
  }
  .settings {
    display: grid;
    grid-template-columns: minmax(0, 1fr);
  }
  textarea {
    width: 100%;
    height: 100%;
  }
  .gogogo {
    display: grid;
    grid-template-columns: minmax(0, 1fr);
  }
  input, textarea {
    background: #000000;
    color: #aaaaaa;
    font-family: courier;
  }
  button {
    background: #000000;
    color: #aaaaaa;
    font-weight: bold;
  }
  label {
    border: 1px solid #444444;
  }
</style>
