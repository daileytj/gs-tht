<script lang="ts">
	import ModeSwitcher from './ModeSwitcher.svelte';
	import Tailwindcss from './Tailwindcss.svelte';
	import Web3 from 'Web3';

	const web3 = new Web3(Web3.givenProvider || "ws://localhost:8545");

	let currentAccount: string;
	let currentChain: string;
	let connectError: string;
	let currentBlockNumber: number;
	let currentGasPrice: string;

	const resetAllData = () => {
		currentAccount = "";
		currentChain = "";
		connectError = "";
		currentBlockNumber = null;
		currentGasPrice = "";
	}

	const connectWallet = async () => {
		  connectError = "";

			try {
				const { ethereum } = window;
				
				if (!ethereum) {
					alert("Get MetaMask!");
					return;
				}

				const accounts = await ethereum.request({
					method: "eth_requestAccounts",
				});
				currentAccount = accounts[0];

				let chainId = await ethereum.request({ method: "eth_chainId" });
				console.log("Connected to chain " + chainId);
				// mainnetChainId = "0x1";
				// ropstenChainId = "0x3";
				// rinkebyChainId = "0x4";
				// goerliChainId = "0x5";
				// kovanChainId = "0x2a";
				
				ethereum.on('chainChanged', handleChainChanged);

				function handleChainChanged(_chainId) {
					window.location.reload();
				}

				switch(chainId) {
					case "0x1":
						currentChain = "Mainnet";
						break;
					case "0x3":
						currentChain = "Ropsten";
						break;
					case "0x4":
						currentChain = "Rinkeby";
						break;
					case "0x5":
						currentChain = "Goerli";
						break;
					case "0x2a":
						currentChain = "Kovan"
						break;
					default: 
						currentChain = "Mainnet";
				}

				web3.eth.getBlockNumber()
					.then((value: number) => {
						currentBlockNumber = value;
					});

				web3.eth.getGasPrice()
					.then((value: string) => {
						currentGasPrice = ((+value / 1000000000).toFixed(2)).toString();
					});

			} catch (error) {
				console.log(error);
				if (error.error && error.error.message) {
					connectError = error.error.message;
				} else {
					connectError = error.message;
				}
			}
		};

		const disconnectWallet = () => {
			resetAllData();

			window.location.reload();
		}
</script>
<style>
	.custom-style {
		@apply italic;
	}
	.card {
		margin-top: 2rem;
		padding: 2rem;
		border-radius: 4px;
		background-color:#27272a;
	}
	.card-body {
		color:#fff;
	}
.btn {
	background-color: #0088f2;
	color: #fff;
	border-radius: 4px;
	padding: 1rem;
	min-width: 250px;
	margin-top: 2rem;
}
.error{
	color: crimson;
	margin-top: 4px;
}
h6 {
	color: #0088f2;
	margin-top: 1rem;
	margin-bottom: .25rem;
	font-size: 1.25rem;
}

</style>
<Tailwindcss />
<ModeSwitcher />
<main class="p-4 mx-auto text-center max-w-xl">
	<h1># Test</h1>
	<p class="custom-style mt-[3rem]">
		Using Svelte, DaisyUI, Tailwind, which are all already set up and ready to go, please create a page that has a button you can click that calls `eth_requestAccounts` or `enable` on the injected provider (Metamask, etc). Once access has been granted to the provider, then display the current ethereum Block Number, Gas Price, and Chain (Goerli, Mainnet, Ropsten, etc). You can use any library you like for this (Web3, Ethers, etc). Typescript is set up and ready to go as well. Google as much as you need. Please try not to spend more than 1 to 2 hours on this.
	</p>
	<div class="container mx-auto">
		<div class="card shadow-xl">
			<div class="card-body">
				{#if !currentAccount}
				<p>Click the fancy blue button below to connect your metamask wallet and display the current ehtereum block number, gas price, and the currently connected chain.</p>
				{/if}
				{#if currentAccount}
				<h6>
					Currently Connected Wallet
				</h6>
				<p>
					{currentAccount}
				</p>
				{/if}
				{#if currentChain}
				<h6>
					Current Chain
				</h6>
				<p>
					{currentChain}
				</p>
				{/if}
				{#if currentBlockNumber}
				<h6>
					Current Block Number
				</h6>
				<p>
					{currentBlockNumber}
				</p>
				{/if}
				{#if currentGasPrice}
				<h6>
					Current Gas Price
				</h6>
				<p>
					{currentGasPrice} gwei
				</p>
				{/if}
				{#if !currentAccount}
				<div class="card-actions justify-end">
					<button class="btn btn-primary" on:click={connectWallet}>Connect</button>
				</div>
				{/if}
				{#if currentAccount}
				<div class="card-actions justify-end">
					<button class="btn btn-primary" on:click={disconnectWallet}>Disconnect</button>
				</div>
				{/if}
				{#if connectError}
					<div class="error">{connectError}</div>
				{/if}
			</div>
		</div>
	</div>
</main>