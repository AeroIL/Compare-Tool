<script>
  import { result } from "./result.js";

  let input1 = "";
  let input2 = "";
  let error1 = "";
  let error2 = "";
  function processInputs() {
    const processed = myProcessingFunction(input1, input2);
    result.set(processed);
  }

  function myProcessingFunction(input1, input2) {
  let parsedInput1, parsedInput2;
  try {
    parsedInput1 = JSON.parse(input1);
  } catch (e) {
    console.error("Invalid JSON string:", e);
    if (e instanceof SyntaxError) {
      error1 = `Error occurred at ${e}`;
    }
  }
  try {
    parsedInput2 = JSON.parse(input2);
  } catch (e) {
    console.error("Invalid JSON string:", e);
    if (e instanceof SyntaxError) {
      error2 =  `Error occurred at ${e}`;
    }
  }
  const comparedResults = {};

  for (let i = 0; i < parsedInput1.ObjectStatus.length; i++) {
    const obj1 = parsedInput1.ObjectStatus[i];
    const className = obj1.Class;

    const obj2 = parsedInput2.ObjectStatus.find(
      (obj) => obj.Class === className && obj.Name === obj1.Name
    );

    if (!obj2) {
      continue;
    }

    const diff = {};
    let isDiff = false;

    for (const key in obj1) {
      if (key === "Class" || key === "Name") {
        continue;
      }
      if (obj1[key] !== obj2[key]) {
        isDiff = true;
        diff[key] = {
          FirstFile: obj1[key],
          SecondFile: obj2[key],
        };
      }
    }

    if (isDiff) {
      if (!comparedResults[className]) {
        comparedResults[className] = [];
      }
      comparedResults[className].push({
        name: obj1.Name,
        Differences: diff,
      });
    }
  }

  // loop through all objects in parsedInput1 and check if it is present in parsedInput2
  for (let i = 0; i < parsedInput1.ObjectStatus.length; i++) {
    const obj1 = parsedInput1.ObjectStatus[i];
    const className = obj1.Class;

    const obj2 = parsedInput2.ObjectStatus.find(
      (obj) => obj.Class === className && obj.Name === obj1.Name
    );

    if (!obj2) {
      const diff = {};
      for (const key in obj1) {
        if (key === "Class" || key === "Name") {
          continue;
        }
        diff[key] = {
          FirstFile: obj1[key],
          SecondFile: "Don't exist",
        };
      }
      if (!comparedResults[className]) {
        comparedResults[className] = [];
      }
      comparedResults[className].push({
        name: obj1.Name,
        Differences: diff,
      });
    }
  }

  return comparedResults;
}

</script>

<main>
  <div class="container">
    <div class="text-area-container">
      <div class="window">
        <textarea name="Left Object" bind:value={input1} class:error={error1.length>0} />
        {#if error1.length >0}
        <p>{error1}</p>
        {/if}
      </div>
      <div class="window">
        <textarea name="Right Object" bind:value={input2}  class:error={error2.length>0}/>
        {#if error2.length >0}
        <p>{error2}</p>
        {/if}
      </div>
    </div>
    <div class="button-container">
      <button on:click={processInputs}>Process</button>
    </div>
  </div>
</main>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    margin: 2rem;
    font-family: sans-serif;
    text-align: center;
    overflow: hidden;
    max-width: 100%;
  }
  .error{
    border: 2px solid red;
  }

  .text-area-container {
    display: flex;
    justify-content: space-evenly;
    width: 100%;
  }

  .window {
    max-width: 600px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 0.5rem;
  }

  textarea {
    display: block;
    box-shadow: inset 0 0 0 1 white;
    width: 30rem;
    height: 30rem;
    margin: 0.5rem;
    padding: 0.5rem;
    border: none;
    border-radius: 0.25rem;
    resize: none;
    background-color: #012456;
    color: #fff;
    font-family: "Lucida Console", Monaco, monospace;
    font-size: 1.1rem;
    line-height: 1.5;
    white-space: pre-wrap;
    overflow-wrap: break-word;
  }

  textarea:focus {
    outline: none;
  }

  ::-webkit-scrollbar {
    width: 10px;
    background-color: #012456;
  }

  ::-webkit-scrollbar-track {
    background-color: #012456;
  }

  ::-webkit-scrollbar-thumb {
    background-color: #fff;
    border-radius: 5px;
  }

  .button-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
  }

  button {
    margin: 0.5rem;
    padding: 0.5rem;
    width: 100%;
    max-width: calc(50% - 1rem);
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 0.25rem;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
  }

  button:hover {
    background-color: rgba(84, 117, 153, 0.468);
  }

  @media only screen and (max-width: 768px) {
    textarea {
      height: 20rem;
    }
    button {
      max-width: 100%;
    }
  }

  @media only screen and (max-width: 576px) {
    textarea {
      height: 15rem;
    }
  }
</style>
