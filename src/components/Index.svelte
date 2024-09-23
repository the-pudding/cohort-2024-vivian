<script>
	import { getContext } from "svelte";
	import { browser } from "$app/environment";
	import Scrolly from "$components/helpers/Scrolly.svelte";
	import viewport from "$stores/viewport.js";
	// import Cat from "$components/Cat.svelte";
	import IPA from "$components/IPA.svelte";
	const copy = getContext("copy");

	let scrollyValue;
	let figureText = "";

	let components = { IPA };
	let currentFigureComponent;

	function updateScrolly() {
		if (scrollyValue === undefined) return;
		const selector = `.step:nth-of-type(${scrollyValue + 1})`;
		const id = document.querySelector(selector).getAttribute("data-id");
		// copy.body.section[1].findIndex(scrollyValue)
		figureText = id;
		currentFigureComponent = components[id];
	}

	$: if (browser) updateScrolly(scrollyValue);
</script>

{#each copy.body as { section, hed, steps }}
	<section id={section}>
		{#if steps}
			<figure>
				<p>{figureText}</p>
				<svelte:component this={currentFigureComponent} />
			</figure>
			<Scrolly bind:value={scrollyValue}>
				{#each steps as { id, text }, i}
					{@const active = scrollyValue === i}
					<div data-id={id} class="step" class:active>
						<p>{text}</p>
					</div>
				{/each}
			</Scrolly>
		{:else}
			<div class="spacer"></div>
			<p>{hed}</p>
			<IPA transcription="miaw"/>
		{/if}
	</section>
{/each}

<style>
	section {
		position: relative;
	}

	.spacer {
		height: 200svh;
	}

	.step {
		padding-bottom: 100svh;
	}

	.step.active p {
		color: red;
	}

	p {
		height: 100px;
	}

	.step:last-of-type p {
		height: 200px;
	}

	figure {
		position: sticky;
		top: 0;
		left: 0;
		width: 100%;
		height: 100svh;
		border: 4px solid blue;
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
