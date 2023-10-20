
<script>
    import { onMount } from "svelte";

    import dayjs from 'dayjs';

    let result;
    let inputValue = 51862; // Initialize with a default value



async function fetchData() {

const apiUrl = `https://api.translink.ca/rttiapi/v1/stops/${inputValue}/estimates?apikey=gF1SoyypUKbaFBiY5jRS`;

// Define headers to request JSON data
const headers = new Headers();
headers.append("Accept", "application/json"); // Set the "Accept" header to request JSON

// Create the request object with the specified headers
const request = new Request(apiUrl, {
  method: "GET",
  headers: headers,
});

// Make the GET request with the custom headers
fetch(request)
  .then(response => {
    // Check if the request was successful (status code 200)
    if (response.ok) {
      // Parse the JSON response
      return response.json();
    } else {
      // Handle the error response
      throw new Error(`API request failed with status: ${response.status}`);
    }
  })
  .then(data => {
    // Handle the JSON data
    console.log("API Response:", data);
    result = data
  })
  .catch(error => {
    // Handle any errors that occurred during the request
    console.error("Error:", error);
  });
}
    onMount(() => {
        fetchData();
    });



let currentTime = dayjs().format('HH:mm:ss');

// Function to update the time every second
const updateTime = () => {
  currentTime = dayjs().format('HH:mm:ss');
};

// Update the time initially and start an interval to update it every second
updateTime();
const interval = setInterval(updateTime, 1000);
</script>
<main class="w-screen mx-auto p-4 h-screen bg-gray-500">

<!-- <h1>Welcome to SvelteKit</h1> -->
<!-- <p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p> -->
<div class="mt-4 p-4 bg-gray-700 rounded">

<!-- <p class="text-2xl">{currentTime}</p> -->
<div class="flex items-center">
    <p class="text-xl mt-1 p-2"> Bus Stop </p>
    <input
      class="w-1/4 border p-2 rounded mr-2"
      type="number"
      bind:value={inputValue}
      on:input={fetchData}
    />
    <button class="bg-orange-500 text-white px-4 py-2 rounded hoverbg-gray-800" on:click={fetchData}>
        GO!
    </button>
</div>



<div>
    {#if result}
        <h2 class="text-xl">Upcoming Buses</h2>
        {#each result as res, i}
        <div class="rounded bg-orange-500 p-4 mb-4">
            <div class="flex">
                    <div class="w-1/2 p-2 ">
                        <div class="text-2xl">Bus {res.RouteNo}</div>
                    </div>
                    <div class="w-1/2 p-2">
                        <div class="text-2xl">{res.Schedules[0].ExpectedCountdown}</div>
                    </div>
                </div>
            <!-- <pre>{JSON.stringify(res, null, 2)}</pre> -->
            </div>
        {/each}
    {/if} 
 </div> 
</div> 


</main>
