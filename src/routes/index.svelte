<script lang="ts">
  import { onMount } from "svelte";
  import GearFill from "$lib/bootstrap-icons/gear-fill.svelte";
  import Preferences from "$lib/components/Preferences.svelte";
  import Quicklinks from "$lib/components/Quicklinks.svelte";
  let time = new Date();
  let prefsOn = false;
  let searchValue = "";
  let twelvehour = false;
  let loading = true;
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
    loading = false;
    console.log(localStorage.getItem("twelvehour"), Boolean(localStorage.getItem("twelvehour")));
    if (localStorage.getItem("quicklinks") == null)
      localStorage.setItem("quicklinks", JSON.stringify(quicklinks));
    if (localStorage.getItem("twelvehour") == null) localStorage.setItem("twelvehour", "false");
    quicklinks = JSON.parse(localStorage.getItem("quicklinks"));
    twelvehour = localStorage.getItem("twelvehour") == "true";
    // update time every 200 ms
    setInterval(() => {
      time = new Date();
      quicklinks = JSON.parse(localStorage.getItem("quicklinks"));
      twelvehour = localStorage.getItem("twelvehour") == "true";
    }, 100);
    // evaluate preferences
    if (window.localStorage.getItem("search-engine") == null)
      window.localStorage.setItem("search-engine", "https://duckduckgo.com/?q=");
  });
</script>

{#if loading}
  <div class="inline text-3xl">
    Loading... <div class="inline-block ml-6 bg-sky-400 w-6 rounded-full animate-ping h-6" />
  </div>
{:else}
  <div id="time-wrapper" class="">
    <h1 class="font-bold text-6xl">
      {#if twelvehour}
        {time.getHours() > 11 ? time.getHours() - 12 : time.getHours()}:{minute}
        {time.getHours() > 10 ? "pm" : "am"}
      {:else}
        {(time.getHours() < 10 ? "0" : "") + time.getHours()}:{minute}
      {/if}
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

  <Quicklinks
    bind:quicklinks
    evalChange={() => window.localStorage.setItem("quicklinks", JSON.stringify(quicklinks))}
  />
  <br />
  <div>
    <button on:click={togglePrefs} class="inline-flex motion-safe:hover:animate-spin">
      <GearFill />
    </button>
    {#if prefsOn}
      <Preferences />
    {/if}
  </div>
{/if}
