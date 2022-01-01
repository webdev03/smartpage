<script lang="ts">
  import { onMount } from "svelte";
  import GearFill from "$lib/bootstrap-icons/gear-fill.svelte";
  import Preferences from "$lib/components/Preferences.svelte";
  let time = new Date();
  let prefsOn = false;
  let searchValue = "";
  const search = () => {
    window.location.href =
      window.localStorage.getItem("search-engine") + encodeURIComponent(searchValue);
  };
  const evalKeyPress = (e) => {
    if (e.keyCode == 13) search();
  };
  const togglePrefs = () => (prefsOn = !prefsOn);
  let quicklinks = [
    {
      name: "Github",
      link: "https://github.com"
    },
    {
      name: "YouTube",
      link: "https://youtube.com"
    },
    {
      name: "Scratch",
      link: "https://scratch.mit.edu"
    },
    {
      name: "Scratch ATs",
      link: "https://scratch.mit.edu/discuss/31"
    }
  ];
  $: hour = time.getHours();
  // Next line taken from https://stackoverflow.com/questions/14529381/leading-zeros-in-minutes#comment77087197_29289639
  $: minute = (time.getMinutes() < 10 ? "0" : "") + time.getMinutes();
  onMount(async () => {
    // update time every 200 ms
    setInterval(() => {
      time = new Date();
    }, 200);
    // evaluate preferences
    if (window.localStorage.getItem("search-engine") == null)
      window.localStorage.setItem("search-engine", "https://duckduckgo.com/?q=");
  });
</script>

<div id="time-wrapper" class="">
  <h1 class="font-bold text-6xl">
    {(time.getHours() < 10 ? "0" : "") + time.getHours()}:{minute}
  </h1>
  <h3>
    {#if hour >= 20}
      Good night!
    {:else if hour >= 17}
      Good evening!
    {:else if hour >= 12}
      Good afternoon!
    {:else}
      Good morning!
    {/if}
  </h3>
</div>

<hr class="mt-3 mb-3 w-96" />

<h1 class="font-bold text-xl">Search the Web</h1>
<div id="input-group" class="flex">
  <input
    on:keypress={evalKeyPress}
    bind:value={searchValue}
    type="text"
    class="rounded-l-md h-12 dark:bg-gray-900 p-2"
    placeholder="Type here..."
  />
  <button class="h-12 rounded-r-md bg-blue-400 p-2" on:click={search}>Go</button>
</div>

<hr class="mt-3 mb-3 w-96" />

<h1 class="mt-2 font-bold text-2xl">Quick Links</h1>
<div class="flex flex-row flex-wrap row-span-3 space-x-2 flex-1 max-w-3xl">
  <div />
  {#each quicklinks as link}
    <a href={link.link} class="hover:font-semibold transition-all">
      <div
        class="inline-block mt-2 p-3 w-36 min-h-96 rounded-md bg-gray-200 dark:bg-gray-900 dark:text-white text-gray-900"
      >
        {link.name}
      </div>
    </a>
  {/each}
</div>

<br />
<div>
  <button on:click={togglePrefs} class="inline-flex hover:animate-spin">
    <GearFill />
  </button>
  {#if prefsOn}
    <Preferences />
  {/if}
</div>
