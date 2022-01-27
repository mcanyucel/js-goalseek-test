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
	let maxStep = 0.1;
	let tolerance = 1;
	let goal = 1525;
	let maxIterations = 1000;
	const fn = (x: number): number => 1.746 * Math.pow(x, 2) - 3.742 * x;
	functionString = fn.toString();

	
	

	onMount(() => {
		solve();
		
	});

	const solve = ():number => {
		errString = "";
		let x = startValue;
		
		const fnParams = [x];

		try {
			result = goalSeek({
				fn: fn,
				fnParams,
				percentTolerance: tolerance,
				maxIterations: maxIterations,
				maxStep: maxStep,
				goal: goal,
				independentVariableIdx: 0
			});	
			fval = fn(result);
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
			<td><input bind:value="{goal}" type="number" step="any" /></td>
		</tr>
		<tr>
			<td>Tolerance %</td>
			<td><input bind:value="{tolerance}" type="number" step="any" /></td>
		</tr>
		<tr>
			<td>Max. Iterations</td>
			<td><input bind:value="{maxIterations}" type="number" step="any" /></td>
		</tr>
		<tr>
			<td>Max. Step Size</td>
			<td><input bind:value="{maxStep}" type="number" step="any" /></td>
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
	</table>
	<button class="btn-block" on:click="{()=> solve()}">Solve</button>
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

	.btn-block {
		display: inline-block;
		min-width: 30%;
		text-align: center;
	}
</style>
