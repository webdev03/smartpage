<script>
  import "$lib/tailwind.css";
</script>

<svelte:head>
  <title>Smartpage | The customisable start page</title>
</svelte:head>

<div class="grid items-center justify-center w-screen min-h-screen dark:bg-gray-800 bg-gray-100 dark:text-gray-100 text-gray-900">
  <div class="p-2">
    <slot />
  </div>
</div>
