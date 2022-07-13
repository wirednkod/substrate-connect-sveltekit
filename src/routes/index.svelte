<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Counter from '$lib/Counter.svelte';


import { createScClient, WellKnownChain } from "connectest";

const all = []
const start = async () => { 
      
  const scClient = createScClient();
  const westendChain = await scClient.addWellKnownChain(
    WellKnownChain.westend2,
    function jsonRpcCallback(response: string) {
      all.push(JSON.stringify(JSON.parse(response)));
      document.getElementById('result').innerText = all.join("\n");
    }
  );
  westendChain.sendJsonRpc(
    '{"jsonrpc":"2.0","id":"1","method":"chainHead_unstable_follow","params":[true]}',
  );
}

start();

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <div id="result"></div>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

  #result {
    margin: 0 auto;
    width: 100%;
    overflow: scroll;
    height: 90vh;
  }

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
