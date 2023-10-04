<script lang="ts">
    import { onMount } from "svelte";
    import * as web3 from '@solana/web3.js';
    import { storeName, publicKey } from './stores';
    import { goto } from '$app/navigation';

    let cnx;
    let invalidKey = false;

    onMount(async () => {
        let sol_rpc = process.env.SOLANA_RPC ? process.env.SOLANA_RPC : "https://api.mainnet-beta.solana.com";
        cnx = new web3.Connection(sol_rpc);
    })

    async function createStore() {
        try {
            $publicKey = $publicKey.trim();

            if (web3.PublicKey.isOnCurve($publicKey) == true) {
                invalidKey = false;
                goto('/store', { state: { foo: 'bar' } });
            } else {
                invalidKey = true;
            }
        } catch (e) {
            invalidKey = true;
        }
    }
</script>

<div class="grid grid-flow-row justify-center">
    <h1 class="sm:pt-3 pt-1 outline font-greycliffbold text-5xl text-center text-transparent bg-clip-text bg-gradient-to-br from-[#20BF55] to-[#01BAEF]">
        <span style="color: #000;">🅰️</span> POS
    </h1>
</div>

<div class="grid grid-flow-row justify-center pt-5 gap-3">
    <h1 class="card-title pt-5 text-center text-md font-greycliffbold -mt-5 pb-2 text-transparent bg-clip-text bg-gradient-to-br from-[#20BF55] to-[#01BAEF]">Create a Point-of-Sale terminal instantly!</h1>
    <div class="indicator justify-items-center place-self-center">
        <div class="card w-96 bg-base-200 shadow border">
            <div class="card-body px-6 pb-4">
                <p class="text-md -mt-5 font-greycliffmed text-transparent bg-clip-text bg-gradient-to-br from-[#20BF55] to-[#01BAEF]">Enter your merchant details</p>
                <input bind:value={$storeName} type="text" placeholder="Store name e.g. Bruno's Burritos" class="input input-sm input-bordered input-accent w-full max-w-xs" />
                <input bind:value={$publicKey} type="text" placeholder="Merchant wallet address (Solana Public Key)" class="input input-sm input-bordered input-accent w-full max-w-xs" />
                {#if invalidKey}
                    <span class="text-center text-sm text-warning">Invalid key - please enter a valid wallet address</span>
                {/if}
                <div class="card-actions justify-center">
                    <button on:click={createStore} class="btn normal-case btn-sm bg-gradient-to-br border-accent hover:border-accent from-[#20BF55] to-[#01BAEF]">Create Store</button>
                    
                <!-- "Buy Solana" button -->
                <a href="https://crypto.link.com?ref=lb&destination_currency=sol&destination_network=solana" class="btn normal-case btn-sm bg-gradient-to-br border-accent hover:border-accent from-[#20BF55] to-[#01BAEF]">Buy Solana</a>

                </div>
            </div>
        </div>
    </div>
</div>

<div class="flex justify-center flex-row pt-2">
    <footer class="footer footer-center p-2 text-base-content rounded-md">
        <div class="items-center">
            <span class="text-sm">POS © 2023</span>
        </div>
    </footer>
</div>
