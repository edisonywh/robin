<script>
  export let id;
  export let index;
  import { fly } from "svelte/transition";
  import { Link } from "svelte-routing";
  import Loader from "./Loader.svelte";

  let endpoint = "https://hacker-news.firebaseio.com/v0";

  let item;

  async function fetchItem(id) {
    const res = await fetch(`${endpoint}/item/${id}.json`);

    let item = await res.json();

    return item;
  }
</script>

<!-- id	The item's unique id.
deleted	true if the item is deleted.
type	The type of item. One of "job", "story", "comment", "poll", or "pollopt".
by	The username of the item's author.
time	Creation date of the item, in Unix Time.
text	The comment, story or poll text. HTML.
dead	true if the item is dead.
parent	The comment's parent: either another comment or the relevant story.
poll	The pollopt's associated poll.
kids	The ids of the item's comments, in ranked display order.
url	The URL of the story.
score	The story's score, or the votes for a pollopt.
title	The title of the story, poll or job.
parts	A list of related pollopts, in display order.
descendants	In the case of stories or polls, the total comment count. -->

{#await fetchItem(id)}
  <Loader />
{:then item}
  <Link to="/item/{id}">
    <div
      in:fly={{ y: 400, duration: 1000, delay: index * 200 }}
      key={id}
      class="card hover:shadow-md font-sans-serif bg-gray-100 shadow p-4 m-4
      rounded">
      <h1 class="font-bold text-xl mb-4">{item.title}</h1>
      <i class="text-sm text-teal-800 block">{new URL(item.url).hostname}</i>
      <i class="text-sm text-teal-800 block my-4">{item.by}</i>
      <div class="flex justify-between">
        <div
          class="flex w-20 justify-around items-center rounded-full bg-teal-100
          text-teal-400 font-bold px-2 py-1">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="#4fd1c5"
            stroke-width="3"
            stroke-linecap="round"
            stroke-linejoin="round">
            <path
              d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />
          </svg>
          {item.descendants}
        </div>
        <div
          class="flex w-20 justify-around items-center rounded-full bg-teal-100
          text-teal-400 font-bold px-2 py-1">
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
            <path
              d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 2-1.7l1.38-9a2 2
              0 0 0-2-2.3zM7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3" />
          </svg>
          {item.score}
        </div>
      </div>
    </div>
  </Link>
{:catch}
  <p>Something is wrong!</p>
{/await}
