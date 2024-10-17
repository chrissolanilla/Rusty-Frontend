<!-- ScoreScreen.svelte -->
<script lang="ts">
	export let players: { name: string, score: number }[]; // Each player has a name and score
</script>

<div class="score-screen">
	<h2>Score Screen</h2>
	<ul>
		{#each players as player}
			<li>{player.name}: {player.score}</li>
		{/each}
	</ul>
</div>

<style>
	.score-screen {
		font-size: 24px;
		color: white;
	}
</style>

