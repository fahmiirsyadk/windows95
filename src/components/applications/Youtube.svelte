<script>
  export let val;
  import Guard from "./../layouts/Guard.svelte";
  import Workspace from "./../layouts/Workspace.svelte";
</script>

<Guard {val}>
  <Workspace id="surprices-app">
    <div class="menu-utama">
      <iframe
        title="video_i_love"
        width="400"
        height="200"
        src="https://www.youtube.com/embed/SX7EZM3363U"
        frameborder="0"
        allow="accelerometer; autoplay; encrypted-media; gyroscope;
        picture-in-picture"
        allowfullscreen />

      <h1>HEY HEY IM ASYNC 2</h1>
    </div>
  </Workspace>
</Guard>
