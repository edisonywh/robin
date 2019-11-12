<script>
  export let id;
  export let indent;

  export let collapseChildren = false;

  import { createEventDispatcher } from "svelte";
  import { fade, fly } from "svelte/transition";
  import Loader from "./Loader.svelte";
  import Kid from "./Kid.svelte";

  let colors = [
    "teal",
    "orange",
    "yellow",
    "green",
    "blue",
    "indigo",
    "purple",
    "pink",
    "gray",
    "red"
  ];

  let endpoint = "https://hacker-news.firebaseio.com/v0";

  let item;
  let color = colors[indent % 11]

  function handleCollapse(event) {
    collapseChildren = !collapseChildren;
    event.stopPropagation();
  }

  async function fetchItem(id) {
    const res = await fetch(`${endpoint}/item/${id}.json`);

    let item = await res.json();

    return item;
  }
</script>

{#await fetchItem(id)}
  <div class="w-screen flex justify-center items-center mt-24">
    <Loader />
  </div>
{:then item}
  {#if item.deleted != true && item.dead != true}
    <div class="flex flex-row w-full">
      <div
        class="mt-2 p-4 w-full bg-white shadow border-l-4 border-{color}-100
        {item.kids ? 'cursor-pointer' : 'cursor-default'}"
        on:click={handleCollapse}
        transition:fade>
        <p class="text-gray-800">
          {@html item.text}
        </p>

        <div class="flex justify-end">
          <i class="text-gray-800 font-bold">{item.by}</i>
        </div>

        {#if item.kids != undefined}
          {#if collapseChildren == true}
            <div
              transition:fade
              class="flex items-center mt-4 bg-teal-100 text-teal-400 py-1 px-2
              font-bold rounded-lg">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill="none"
                stroke="#4fd1c5"
                stroke-width="3"
                stroke-linecap="butt"
                stroke-linejoin="round">
                <path d="M6 9l6 6 6-6" />
              </svg>
              <p class="pl-2">Collapsed ({item.kids.length})</p>
            </div>
          {:else}
            {#each item.kids as kid}
              <div class="flex flex-row">
                <Kid id={kid} indent={indent + 1} />
              </div>
            {/each}
          {/if}
        {/if}
      </div>
    </div>
  {:else}
    <div
      class="mt-2 p-3 text-yellow-400 w-full bg-yellow-100 shadow border-l-4
      border-yellow-400 font-bold">
      Deleted
    </div>
  {/if}
{/await}
