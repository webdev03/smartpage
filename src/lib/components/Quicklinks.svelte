<script lang="ts">
  import PencilSquare from "$lib/bootstrap-icons/pencil-square.svelte";
  export let quicklinks;
  export let evalChange = () => {};
  let editing = false;
</script>

<h1 class="inline mt-2 font-bold text-2xl">Quick Links</h1> 
<button class="ml-2" on:click={() => editing = !editing}><PencilSquare /></button>
<div class="flex flex-row flex-wrap row-span-3 space-x-2 flex-1 max-w-4xl overflow-scroll">
  <div />
  {#each quicklinks as link}
    {#if editing}
    <div class="hover:font-semibold transition-all">
      <div
        class="inline-block mt-2 p-3 w-48 min-h-96 rounded-md bg-gray-200 dark:bg-gray-900 dark:text-white text-gray-900"
      >
        <input type="text" class="w-36 bg-transparent" bind:value={link.name} on:input={evalChange}>
        <input type="text" class="w-36 bg-transparent" bind:value={link.link} on:input={evalChange}>
      </div>
    </div>
    {:else}
    <a href={link.link} class="hover:font-semibold transition-all">
      <div
        class="inline-block mt-2 p-3 w-36 min-h-96 rounded-md bg-gray-200 dark:bg-gray-900 dark:text-white text-gray-900"
      >
        {link.name}
      </div>
    </a>
    {/if}
  {/each}
</div>