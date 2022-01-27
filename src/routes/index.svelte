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

	<table>
		<tr>
			<td>Function</td>
			<td>{functionString}</td>
		</tr>
		<tr>
			<td>Goal</td>
			<td>{goal}</td>
		</tr>
		<tr>
			<td>Result</td>
			{#if errString !==""}
				<td>Could not solve: {errString}</td>
			{:else}
				<td>{result.toFixed(3)}</td>
			{/if}
		</tr>
		<tr>
			<td>Function value</td>
			{#if errString !==""}
				<td>Could not solve: {errString}</td>
			{:else}
			<td>{fval.toFixed(3)}</td>
			{/if}
		</tr>
		<tr>
			<td>Result L1 Norm (Error)</td>
			{#if errString !==""}
				<td>Could not solve: {errString}</td>
			{:else}
			<td>{((goal-fval)/goal * 100).toFixed(2)}%</td>
			{/if}
		</tr>
		<tr>
			<td>Tolerance</td>
			<td>{tolerance}</td>
		</tr>
		<tr>
			<td>Max. Iterations</td>
			<td>{maxIterations}</td>
		</tr>
		<tr>
			<td>Max. Step Size</td>
			<td>{maxStep}</td>
		</tr>
	</table>
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
	table {
	font-family: arial, sans-serif;
	border-collapse: collapse;
	width: 100%;
	}

	td {
	border: 1px solid #dddddd;
	text-align: left;
	padding: 8px;
	}	

	td:nth-child(odd) {
		font-weight: bold;
	}
</style>
