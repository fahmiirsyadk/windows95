<script>
  export let toggleAccess;
  import routes from "./routes.js";

  const toggleEffect = name => {
    toggleAccess(name);
  };
</script>

<style>
  .menu-panel {
    border-top: 1px solid silver;
    border-left: 1px solid silver;
    background-color: silver;
    width: 200px;
    display: none;
  }

  .menu-panel--show {
    border-top: 1px solid silver;
    border-left: 1px solid silver;
    background-color: silver;
    width: 200px;
    display: block !important;
    position: absolute;
    bottom: 28px;
  }

  .menu-panel ul {
    color: #000;
    border-top: 1px solid #fff;
    border-left: 1px solid #fff;
    border-right: 1px solid gray;
    border-bottom: 1px solid gray;
    box-shadow: inset 1px 1px #dfdfdf, 1px 0 #000, 0 1px #000, 1px 1px #000;
    background-color: silver;
  }

  .menu-panel ul li {
    font-size: 8px;
    color: #000;
    padding: 3px;
    cursor: pointer;
    display: flex;
  }

  .menu-panel ul li:hover {
    background-color: #0090e4;
    color: #fff;
    text-shadow: 0 0 transparent !important;
  }
</style>

<div class="menu-panel menu-panel--show">
  <ul id="menu-panel-items">
    {#each routes as application}
      {#if !application.system}
        <li on:click={() => toggleEffect(application.name)}>
          {application.name}
        </li>
      {/if}
    {/each}
  </ul>
</div>
