<script>
  import Card from "./Card.svelte";
  import Loader from "./Loader.svelte";

  let endpoint = "https://hacker-news.firebaseio.com/v0";

  let storiesList = [];

  async function fetchTopStories() {
    const res = await fetch(`${endpoint}/topstories.json`);

    return await res.json();
  }
</script>

{#await fetchTopStories()}
  <div class="h-screen w-screen flex justify-center items-center">
    <Loader />
  </div>
{:then stories}
  {#each stories.slice(0, 20) as story, index}
    <div class="container max-w-4xl mx-auto">
      <Card id={story} {index} />
    </div>
  {/each}
{:catch}
  <p>Error!</p>
{/await}
