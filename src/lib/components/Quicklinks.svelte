<script lang="ts">
  import PencilSquare from "$lib/bootstrap-icons/pencil-square.svelte";
  import Plus from "$lib/bootstrap-icons/plus.svelte";
  import CrossX from "$lib/bootstrap-icons/x.svelte";
  import { onMount } from "svelte";
  export let quicklinks;
  export let evalChange = () => {};
  let editing = false;
  let removeQuicklink;
  const createQuicklink = () => {
    quicklinks.push({ name: "My Quicklink", link: "https://example.com" });
    window.localStorage.setItem("quicklinks", JSON.stringify(quicklinks));
    quicklinks = quicklinks;
  };
  onMount(() => {
    removeQuicklink = (i) => {
      if (quicklinks.length === 1) return;
      if (!confirm("Are you sure you want to delete this quick link?")) return;
      quicklinks.splice(i, 1);
      window.localStorage.setItem("quicklinks", JSON.stringify(quicklinks));
      quicklinks = quicklinks;
    };
  });
</script>

<h1 class="inline mt-2 font-bold text-2xl">Quick Links</h1>
<button class="ml-2" on:click={() => (editing = !editing)}><PencilSquare /></button>
<div
  class="justify-center flex flex-row flex-wrap row-span-3 space-x-2 flex-1 max-w-4xl overflow-hidden"
>
  <div />
  {#if editing}
    {#each quicklinks as link, i}
      <div class="hover:font-semibold transition-all text-left">
        <div
          class="inline-block mt-2 p-3 w-48 min-h-96 rounded-md bg-gray-200 dark:bg-gray-900 dark:text-white text-gray-900"
        >
          <input
            type="text"
            class="w-full m-0.5 bg-transparent"
            bind:value={link.name}
            on:input={evalChange}
          />
          <input
            type="text"
            class="w-full m-0.5 bg-transparent"
            bind:value={link.link}
            on:input={evalChange}
          />
          <div class="w-full"></div>
          <button on:click={removeQuicklink(i)}><CrossX /></button>
        </div>
      </div>
    {/each}
    <div class="w-full" />
    <button class="motion-safe:hover:animate-pulse" on:click={createQuicklink}><Plus /></button>
  {:else}
    {#each quicklinks as link}
      <a href={link.link} class="hover:font-semibold transition-all">
        <div
          class="inline-block mt-2 p-3 w-36 min-h-96 rounded-md bg-gray-200 dark:bg-gray-900 dark:text-white text-gray-900"
        >
          {link.name}
        </div>
      </a>
    {/each}
  {/if}
</div>
