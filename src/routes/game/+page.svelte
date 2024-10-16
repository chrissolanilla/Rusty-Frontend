<script lang="ts">
	import { onMount } from 'svelte';
	let players:String[] = ["test1", "test2", "test3"];
	let hasJoined = false;
	let username = '';
	let socket: WebSocket;

	function checkPlayersArray() {
		console.log(players);
	}

	function removePlayer() {
		players = [...players.slice(0, -1)];
	}

	onMount(() => {
		//socket logic to get the current players
		const backendUrl = import.meta.env.VITE_BACKEND_URL || 'ws://localhost:3000/ws';
		socket = new WebSocket(backendUrl);

		socket.addEventListener('message', (event) => {
			const message = event.data;

			if (message.includes('joined the lobby') || message.includes('has left the lobby')) {
				// Update the players list when someone joins or leaves
				updatePlayersList(message);
			} else {
				console.log('Message from server:', message);
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
		// send the username as the first message to join the game
		socket.send(username);
		hasJoined = true;
    }

	function updatePlayersList(message: string) {
		const [player, action] = message.split(' has ');

		if (action === 'joined the lobby') {
			if (!players.includes(player)) {
				players = [...players, player]; // Create a new array to trigger reactivity
			}
		} else if (action === 'left the lobby') {
			players = players.filter((p) => p !== player); // Create a new array to trigger reactivity
		}
	}


</script>

<div class="container">
	<h1>Welcome to Rusty Reef Waiting Room</h1>
	<label for="usernameInput">Enter a username and click join to join the game</label>
	<input id="usernameInput" type="text" bind:value={username} disabled={hasJoined} />
	<button on:click={joinGame} disabled={hasJoined}>Join Game</button>
	<div class="playersContainer">
		<h2>Players in the game:</h2>
		<ul>
			{#each players as player}
				<li>{player}</li>
			{/each}
		</ul>
	</div>
	<button on:click={checkPlayersArray}>Check Players</button>
	<button on:click={removePlayer}>Remove Player</button>
</div>

<style>
	:global(body) {
		margin: 0;
	}
    .container {
		display: flex;
		justify-content: center;
    }
</style>
