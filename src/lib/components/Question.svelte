<!-- Question.svelte -->
<script lang="ts">
	export let currentQuestion: string;
	export let answers: string[]; // A list of answer choices
</script>

<div class="question">
	<h2>{currentQuestion}</h2>
	<div class="multiple-choice-container">
		{#each answers as answer, index}
			<input type="radio" name="answer" value={index + 1} id={"answer" + (index + 1)} />
			<label for={"answer" + (index + 1)}>{answer}</label>
		{/each}
	</div>
</div>

<style>
	.question {
		font-size: 24px;
		color: white;
	}
</style>

