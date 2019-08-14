<script>
  import Nav from "../components/Nav.svelte";

  export let segment;
</script>

<style>
  .desktop-view {
    background-size: cover;
    background-color: #2e6e6d;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    position: relative;
  }
</style>

<Nav {segment} />

<main class="desktop-view">
  <slot />
</main>
