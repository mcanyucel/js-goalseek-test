<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import goalSeek from 'goal-seek';
	import { onMount } from 'svelte';

	let functionString: string;
	let result = -1;
	let fval = -1;
	let errString = "";
	const startValue = 20;
	const maxStep = 0.1;
	const tolerance = 1;
	const goal = 1476.32;
	const maxIterations = 1000;
	const fn = (x: number): number => 1.746 * Math.pow(x, 2) - 3.742 * x;

	functionString = fn.toString();

	
	

	onMount(() => {
		result = solve();
		fval = fn(result);
	});

	const solve = ():number => {
		errString = "";
		let x = startValue;
		
		const fnParams = [x];

		try {
			return goalSeek({
				fn,
				fnParams,
				percentTolerance: tolerance,
				maxIterations: maxIterations,
				maxStep: maxStep,
				goal: goal,
				independentVariableIdx: 0
			});	
		} catch (e) {
			errString = e.message;
			console.log(errString);
			
			return -1;
		}
	};
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<section>
	<h1>
		<div class="welcome">
			<picture>
				<source srcset="svelte-welcome.webp" type="image/webp" />
				<img src="svelte-welcome.png" alt="Welcome" />
			</picture>
		</div>

		to js-goalseek test app.
	</h1>

	<h2>
		The target value of function <p>{functionString}</p> for goal <p>{goal}</p> is <p>{result.toFixed(3)}</p> with a function value of <p>{fval.toFixed(3)}</p>The error is <p>{((goal-fval)/goal * 100).toFixed(2)}%</p>
	</h2>

	{#if errString !==""}
		<p>Could not solve: {errString}</p>
	{/if}

	<h2>
		Tolerance: {tolerance}% - Max. Iterations: {maxIterations} - Max. Step: {maxStep}
	</h2>

	
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	h1 {
		width: 100%;
	}

	p {
		color: red;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.welcome {
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
