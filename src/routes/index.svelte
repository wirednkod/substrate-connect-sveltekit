<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Counter from '$lib/Counter.svelte';


import { ScProvider, WellKnownChain } from "@polkadot/rpc-provider/substrate-connect";
import { ApiPromise } from '@polkadot/api';

const all = []
const start = async () => { 

  const provider = new ScProvider(WellKnownChain.polkadot);
  await provider.connect(); 
  // Create the PolkadotJS api instance
  const api = await ApiPromise.create({ provider });
  await api.rpc.chain.subscribeNewHeads((lastHeader) => {
    console.log(lastHeader.hash.toHuman());
  });
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
