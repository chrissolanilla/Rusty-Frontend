<div class="container">
	<h1>Welcome to the Rusty Reef</h1>
	<div class="imageContainer">
		<img src="crab-rave.gif" alt="crab rave gif" class="gif" >
	</div>
	<h2>Lets test how rusty you are</h2>
	<a href="/game" ><button >Join now</button></a>
</div>

<style lang="scss">
	:global(body) {
		margin: 0;
				background-image: url('rustyReef.jpg');
		background-repeat: no-repeat;
		background-size: cover;

		}
	* {
		text-align: center;
		color: red;
	}
	.container {
		background-image: url('rustyReef.jpg');
		background-repeat: no-repeat;
		background-size: cover;
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
