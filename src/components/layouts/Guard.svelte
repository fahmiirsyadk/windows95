<script>
  export let val;
</script>

{#if val}
  <div style="top: 100px; position: absolute;">
    <slot />
  </div>
{/if}
<!-- this component used to load or unload the application -->
