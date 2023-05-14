<script>
    import { result } from "./result.js";
    
    let selectedOption = ""; // initialize selectedOption to an empty string
    
    const handleReset = () => {
      result.set(null);
      selectedOption = ""; // reset selectedOption to an empty string
    };
    
    const handleOptionSelect = (event) => {
      selectedOption = event.target.value; // update selectedOption with the selected value
    };
    
    let data;
    result.subscribe((value) => {
      data = value;
    });
  </script>
  
  <div class="card">
    <button on:click={handleReset}>Reset</button>
    <select on:change={handleOptionSelect}>
      <option value="">All Objects</option>
      {#each Object.keys(data) as key}
        <option value={key}>{key}</option>
      {/each}
    </select>
    {#if selectedOption === ""} 
      {#each Object.keys(data) as key}
        <h2>{key}</h2>
        {#each data[key] as item}
          <div class="item">
            <p>{item.name}</p>
            {#each Object.keys(item.Differences) as diff}
            {#if diff != "Not found in second Object Status"}
              <p>{diff}: {item.Differences[diff].FirstFile !== "" ? item.Differences[diff].FirstFile : "None"} ≠ {item.Differences[diff].SecondFile !== "" ? item.Differences[diff].SecondFile : "None"}</p>
            {:else}
            <p>Object dosen't exist in second input</p>
            {/if}
              {/each}
          </div>
        {/each}
      {/each}
    {:else}
      <h2>{selectedOption}</h2>
      {#each data[selectedOption] as item}
        <div class="item">
          <p>{item.name}</p>
          {#each Object.keys(item.Differences) as diff}
            <p>{diff}: {item.Differences[diff].FirstFile !== "" ? item.Differences[diff].FirstFile : "None"} ≠ {item.Differences[diff].SecondFile !== "" ? item.Differences[diff].SecondFile : "None"}</p>
          {/each}
        </div>
      {/each}
    {/if}
  </div>
  
  <style>
    .card {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 2rem;
      background-color: #fff;
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
      border-radius: 8px;
      width: 100%;
      max-width: 800px;
    }
  
    .card button {
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 5px;
      background-color: #4CAF50;
      color: #fff;
      font-size: 1.1rem;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s ease-in-out;
      margin-bottom: 1rem;
    }
  
    .card button:hover {
      background-color: #3e8e41;
      transform: translateY(-3px);
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
    }
  
    .card h2 {
      font-size: 2rem;
      color: #555;
      text-align: center;
      margin-bottom: 1rem;
    }
  
    .item {
      background-color: #f5f5f5;
      border-radius: 8px;
      padding: 1rem;
      margin-bottom: 1rem;
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
      transition: all 0.3s ease-in-out;
      width: 100%;
      max-width: 600px;
    }
  
    .item:hover {
      transform: translateY(-3px);
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
    }
  
    .item p {
      font-size: 1rem;
      color: #555;
      margin-bottom: 0.5rem;
      line-height: 1.4;
    }
  
    .item p:first-child {
      font-size: 1.2rem;
      font-weight: bold;
      margin-bottom: 1rem;
    }
  
    .item p:last-child {
      margin-bottom: 0;
    }
  </style>
  