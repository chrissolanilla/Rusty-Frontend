<div class="container">
	<h1>Welcome to the Rusty Reef</h1>
	<div class="imageContainer">
		<img src="crab-rave.gif" alt="crab rave gif" class="gif" >
	</div>
	<h2>Lets test how rusty you are</h2>
	<a href="/game" ><button >Join now</button></a>
</div>

<style lang="scss">
	:gloabl(body){
		margin:0;
		background: blue;
	}

	* {
		text-align: center;
		color: red;
	}

	.container {
		display: flex;
		flex-direction: column;
		justify-content: center;

		.imageContainer {
			display: flex;
			justify-content: center;
		}
	}

	.gif {
		width:30%;
		object-fit:cover;
		object-position: 0 -150px;
		height: auto;
	}
</style>
