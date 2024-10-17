<script lang="ts">
    import { onMount } from 'svelte';
    import Question from "$lib/components/Question.svelte";
    import Scorescreen from "$lib/components/Scorescreen.svelte";

    //game state
    let players = [];
    let hasJoined = false;
    let username = '';
    let socket: WebSocket;
    let gameStarted = false;
    let scoreScreenIsShowing = false;
    let currentQuestion = '';
    let answers = ["Paris", "London", "Berlin", "Rome"]; // Example answers for a question

    function checkPlayersArray() {
        console.log(players);
    }

    function fetchPlayers() {
        socket.send(JSON.stringify({ type: 'get_players' }));
    }

    onMount(() => {
        const backendUrl = import.meta.env.VITE_BACKEND_URL || 'ws://localhost:3000/ws';
        socket = new WebSocket(backendUrl);

        socket.addEventListener('open', () => {
            console.log("WebSocket connection opened.");
        });

        socket.addEventListener('message', (event) => {
            const data = JSON.parse(event.data);
            if (data.players) {
                players = data.players; // Update players with the list from the server
            }
        });

        socket.addEventListener('close', () => {
            hasJoined = false;
            alert('Disconnected from the server');
        });
    });

    function joinGame() {
        if (username.trim() === '') {
            alert('Please enter a username');
            return;
        }
        // Send the username as the first message to the server
		socket.send(JSON.stringify({ action_type: 'join_game', username }));
        hasJoined = true;
        fetchPlayers();
    }
</script>

{#if !gameStarted}
<div class="container">
    <h1>Welcome to Rusty Reef Waiting Room</h1>

    <form class="form">
        <label for="usernameInput">Enter a username and click join to join the game</label>
        <input id="usernameInput" type="text" bind:value={username} disabled={hasJoined} />
        <button on:click={joinGame} disabled={hasJoined}>Join Game</button>
    </form>

    <div class="playersContainer">
        <h2>Players in the game:</h2>
        <ul>
            {#each players as player}
                <li>{player.name}</li>
            {/each}
        </ul>
    </div>
</div>
{:else if gameStarted && scoreScreenIsShowing}
    <Scorescreen players={players} />
{:else}
    <Question currentQuestion={currentQuestion} answers={answers}/>
{/if}

<style>
    :gloabl(body) {
        margin: 0;
        background: linear-gradient(#017de6, #015eea);
    }

    .container {
        display: flex;
        justify-content: center;
        text-align: center;
        flex-direction: column;
    }
</style>

