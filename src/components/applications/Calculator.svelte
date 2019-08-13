<script>
  export let val;
  import Guard from "../layouts/Guard.svelte";
  import Workspace from "../layouts/Workspace.svelte";

  let calculator;
  let displayedNum = 0;

  const calculate = (n1, operator, n2) => {
    const firstNum = parseFloat(n1);
    const secondNum = parseFloat(n2);
    if (operator === "add") return firstNum + secondNum;
    if (operator === "subtract") return firstNum - secondNum;
    if (operator === "multiply") return firstNum * secondNum;
    if (operator === "divide") return firstNum / secondNum;
  };

  const getKeyType = key => {
    const { action } = key.dataset;
    if (!action) return "number";
    if (
      action === "add" ||
      action === "subtract" ||
      action === "multiply" ||
      action === "divide"
    )
      return "operator";
    // For everything else, return the action
    return action;
  };

  const createResultString = (key, displayedNum, state) => {
    const keyContent = key.textContent;
    const keyType = getKeyType(key);
    const { firstValue, operator, modValue, previousKeyType } = state;

    if (keyType === "number") {
      return displayedNum === 0 ||
        previousKeyType === "operator" ||
        previousKeyType === "calculate"
        ? keyContent
        : displayedNum + keyContent;
    }

    if (keyType === "decimal") {
      if (!displayedNum.includes(".")) return displayedNum + ".";
      if (previousKeyType === "operator" || previousKeyType === "calculate")
        return "0.";
      return displayedNum;
    }

    if (keyType === "operator") {
      return firstValue &&
        operator &&
        previousKeyType !== "operator" &&
        previousKeyType !== "calculate"
        ? calculate(firstValue, operator, displayedNum)
        : displayedNum;
    }

    if (keyType === "clear") return 0;

    if (keyType === "calculate") {
      return firstValue
        ? previousKeyType === "calculate"
          ? calculate(displayedNum, operator, modValue)
          : calculate(firstValue, operator, displayedNum)
        : displayedNum;
    }
  };

  const updateCalculatorState = (
    key,
    calculator,
    calculatedValue,
    displayedNum
  ) => {
    const keyType = getKeyType(key);
    const {
      firstValue,
      operator,
      modValue,
      previousKeyType
    } = calculator.dataset;

    calculator.dataset.previousKeyType = keyType;

    if (keyType === "operator") {
      calculator.dataset.operator = key.dataset.action;
      calculator.dataset.firstValue =
        firstValue &&
        operator &&
        previousKeyType !== "operator" &&
        previousKeyType !== "calculate"
          ? calculatedValue
          : displayedNum;
    }

    if (keyType === "calculate") {
      calculator.dataset.modValue =
        firstValue && previousKeyType === "calculate" ? modValue : displayedNum;
    }

    if (keyType === "clear" && key.textContent === "AC") {
      calculator.dataset.firstValue = "";
      calculator.dataset.modValue = "";
      calculator.dataset.operator = "";
      calculator.dataset.previousKeyType = "";
    }
  };

  const updateVisualState = (key, calculator) => {
    const keyType = getKeyType(key);
    Array.from(key.parentNode.children).forEach(k =>
      k.classList.remove("is-depressed")
    );

    if (keyType === "operator") key.classList.add("is-depressed");
    if (keyType === "clear" && key.textContent !== "AC") key.textContent = "AC";
    if (keyType !== "clear") {
      const clearButton = calculator.querySelector("[data-action=clear]");
      clearButton.textContent = "CE";
    }
  };

  const getStroke = e => {
    const key = e.target;
    const resultString = createResultString(
      key,
      displayedNum,
      calculator.dataset
    );

    displayedNum = resultString;
    updateCalculatorState(key, calculator, resultString, displayedNum);
    updateVisualState(key, calculator);
  };
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
    <div class="calculator" bind:this={calculator}>
      <div class="calculator__display">{displayedNum}</div>
      <div class="calculator__keys" on:click={getStroke}>
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
