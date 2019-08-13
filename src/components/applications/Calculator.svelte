<script>
  export let val;
  import Guard from "../layouts/Guard.svelte";
  import Workspace from "../layouts/Workspace.svelte";
</script>

<style>
  .calculator {
    border-radius: 12px;
    box-shadow: 0 0 40px 0px rgba(0, 0, 0, 0.15);
    margin-left: auto;
    margin-right: auto;
    margin-top: 2em;
    max-width: 15em;
    overflow: hidden;
  }

  .calculator__display {
    background-color: #222222;
    color: #fff;
    font-size: 1.714285714em;
    padding: 0.5em 0.75em;
    text-align: right;
  }

  .calculator__keys {
    background-color: #999;
    display: grid;
    grid-gap: 1px;
    grid-template-columns: repeat(4, 1fr);
  }

  .calculator__keys > * {
    background-color: #fff;
    padding: 0.5em 1.25em;
    position: relative;
    text-align: center;
  }

  .calculator__keys > *:active::before,
  .calculator__keys > .is-depressed::before {
    background-color: rgba(0, 0, 0, 0.2);
    bottom: 0;
    box-shadow: 0 0 6px 0 rgba(0, 0, 0, 0.5) inset;
    content: "";
    left: 0;
    opacity: 0.3;
    position: absolute;
    right: 0;
    top: 0;
    z-index: 1;
  }

  .key--operator {
    background-color: #eee;
  }

  .key--equal {
    background-image: linear-gradient(to bottom, #fe886a, #ff7033);
    grid-column: -2;
    grid-row: 2 / span 4;
  }
</style>

<Guard {val}>
  <Workspace id="calculator-app">
    <div class="calculator">
      <div class="calculator__display">0</div>
      <div class="calculator__keys">
        <button class="key--operator" data-action="add">+</button>
        <button class="key--operator" data-action="subtract">-</button>
        <button class="key--operator" data-action="multiply">&times;</button>
        <button class="key--operator" data-action="divide">÷</button>
        <button>7</button>
        <button>8</button>
        <button>9</button>
        <button>4</button>
        <button>5</button>
        <button>6</button>
        <button>1</button>
        <button>2</button>
        <button>3</button>
        <button>0</button>
        <button data-action="decimal">.</button>
        <button data-action="clear">AC</button>
        <button class="key--equal" data-action="calculate">=</button>
      </div>
    </div>
  </Workspace>
</Guard>
