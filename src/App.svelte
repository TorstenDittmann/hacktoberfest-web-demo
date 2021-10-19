<script lang="ts">
    import { Appwrite } from "appwrite";
    import { onMount } from "svelte";
    import { LogoHacktoberfest } from "./assets/logos";
    import Partners from "./lib/Partners.svelte";

    const COLLECTION = "616ec59046be7";
    const reactions = {
        1: "🚀",
        2: "👍",
        3: "❤️",
        4: "🐙",
        5: "⚽️",
    };

    let logs = [];

    const sdk = new Appwrite();

    sdk.setEndpoint("https://demo.appwrite.io/v1").setProject("616ec469790ef");

    onMount(async () => {
        try {
            await sdk.account.get();
        } catch (error) {
            await sdk.account.createAnonymousSession();
        }

        sdk.subscribe<{ emoji: number }>(
            "collections." + COLLECTION + ".documents",
            (event) => {
                if (logs.length >= 144) {
                    logs.pop();
                }
                logs = [event.payload.emoji, ...logs];
            }
        );
    });

    const sendReaction = async (reaction: number) => {
        try {
            await sdk.database.createDocument(
                COLLECTION,
                {
                    emoji: reaction,
                },
                ["*"]
            );
        } catch (error) {
            console.log(error);
        }
    };
</script>

<main>
    <img class="logo" src={LogoHacktoberfest} alt="HacktoberFest Logo" />
    <Partners />

    <div class="game">
        <div class="controls">
            <button on:click={() => sendReaction(1)}>{reactions[1]}</button>
            <button on:click={() => sendReaction(2)}>{reactions[2]}</button>
            <button on:click={() => sendReaction(3)}>{reactions[3]}</button>
            <button on:click={() => sendReaction(4)}>{reactions[4]}</button>
            <button on:click={() => sendReaction(5)}>{reactions[5]}</button>
        </div>
        <div class="log">
            {#each logs as log}
                <span>{reactions[log]}</span>
            {/each}
        </div>
    </div>
</main>

<style>
    :root {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
            Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
        background-color: #2b3531;
        color: #f4f0e1;
    }

    :global(body) {
        margin: 0;
    }

    main {
        text-align: center;
        margin: 0 auto;
        height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .logo {
        max-width: 24rem;
    }

    .game {
        display: flex;
        width: 30rem;
        gap: 1rem;
    }

    .game .controls {
        display: flex;
        flex-direction: column;
        width: 4rem;
    }

    .game .controls button {
        width: 4rem;
        height: 4rem;
        background: none;
        border: none;
        font-size: 2.5rem;
        cursor: pointer;
        transition: all 0.2s ease;
    }

    .game .controls button:hover {
        font-size: 3rem;
    }

    .game .controls button:active {
        transform: rotate(20deg);
    }

    .game .log {
        background-color: #4f5b4c;
        width: 100%;
        border-radius: 1rem;
    }

    .game .log span {
        margin: 0.5rem;
    }
</style>
