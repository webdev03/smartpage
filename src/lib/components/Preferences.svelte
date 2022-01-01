<script>
  let engine = window.localStorage.getItem("search-engine");
  const setEngine = () => {
    window.localStorage.setItem("search-engine", engine)
  } 
</script>

<div
	class="absolute top-3 justify-center left-0 right-0 ml-auto mr-auto shadow-lg w-96 min-h-[16rem] dark:bg-gray-900 bg-gray-200 rounded p-2"
>
	<h1 class="font-bold text-2xl">Preferences</h1>
  <hr class="mt-2 mb-2">
  <label for="engine">Search engine: </label>
  <input type="text" class="dark:text-gray-900 rounded p-1" on:change={setEngine} bind:value={engine}>
  <p class="text-sm">At the end of this URL is where your search query will go</p>
</div>