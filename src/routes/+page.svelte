<script>
    import { RingLoader } from 'svelte-loading-spinners';
    let query = '';
    let answer = '';
    let loading = false;
    let language = 'bash';

    const clearSubmit = async () => {
        answer = ''
        query = ''
        loading = false
    }
    
    const handleSubmit = async (event) => {
        event.preventDefault();
        loading = true;
        
        const response = await fetch('https://api.openai.com/v1/completions', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer '+import.meta.env.VITE_OPENAPI_SDK_KEY
            },
            body: JSON.stringify({
                model: "text-davinci-003",
                prompt: query,
                max_tokens: 4000,
                temperature: 0.5
            })
        });
        const json = await response.json();
        answer = json.choices[0].text
        console.log(answer)
        loading = false

    }

</script>

<section class="flex justify-center items-center text-transparent text-7xl bg-clip-text bg-gradient-to-r from-yellow-500 to-orange-800 col-span-3 row-start-1">
    SvelteJS and ChatGPT API
</section>

<section class="col-span-3 row-start-2 row-span-2 h-5/6 p-6 bg-gray-900 shadow-2xl w-1/2 mx-auto overflow-y-auto">
    <div class="text-center pb-2">
        <form on:submit|preventDefault={handleSubmit}>
            <input class='w-2/3 border-b-4 text-l bg-gray-900 border-yellow-500 text-white' type="text" bind:value={query} placeholder="Enter Your ChatGPT Prompt Here"/>
            <button class='border-2 border-yellow-500 text-l m-2 p-2 w-1/6 text-white' type="submit">Submit</button>
            
        </form>
        <button class='border-2 border-red-500 text-l m-2 p-2 w-1/5 text-white' on:click={clearSubmit}>Clear Results</button>
    </div>
    {#if loading} 
    <div class='flex flex-row items-center justify-center pt-4'>
        <RingLoader size="100" color="#fbc500" unit="px" duration="1s" />
    </div>
    {:else}
    <div class=' text-white text-l text-left whitespace-pre-wrap'>
        {answer}
    </div>
    {/if}
</section>

