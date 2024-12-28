<script>
	import { getContext } from "svelte";
	import { browser } from "$app/environment";
	import { blur } from 'svelte/transition';
	import Scrolly from "$components/helpers/Scrolly.svelte";
	import viewport from "$stores/viewport.js";
	import Cat from "$components/Cat.svelte";
	import Pig from "$components/Pig.svelte";
	import Duck from "$components/Duck.svelte";
	import IPA from "$components/IPA.svelte";

	const copy = getContext("copy");
	let body = copy.body;
	let hedArr = body.intro.hed.split(" ");

	// scrolly variables
	let scrollyValue;
	let figureText = "";
	let components = {cat: Cat, pig: Pig, duck: Duck};
	let currentFigureComponent = components["cat"];
	let currentFigureComponentProps;

	function updateScrolly() {
		if (scrollyValue === undefined) return;
		const selector = `.step:nth-of-type(${scrollyValue + 1})`;
		const id = document.querySelector(selector).getAttribute("data-id");
		figureText = id;
		// if (currentFigureComponent != components[id]) {
		currentFigureComponent = components[id];
		// } 
		currentFigureComponentProps = body.scrolly.steps[scrollyValue].props;
	}

	$: if (browser) updateScrolly(scrollyValue);
</script>

<section id="intro">
	<div class="cover">
		<div class="cover-content">
			<div class="hed">
				<p class="hed-front">{hedArr[0]}</p>
				<p class="hed-back">{hedArr[1]}</p>
			</div>
			<p class="subhed">{body.intro.subhed}</p>
			<div class="byline">by <a href={body.intro.bylineLink}>{body.intro.byline}</a></div>
		</div>
	</div>
	<div class="body-content">
		{#each body.intro.content as content}
		 	{#if content.type === "text"}
			<p class="graf">{@html content.value}</p>
			{:else if content.type === "component"}
				{#if content.value === "ipaCatExample"}
				<div style="padding: 1.5em"><IPA ipa="miaw" word="meow" lang="english" ipaScale=2/></div>
				{/if}
			{/if}
		{/each}
	</div>
</section>

<section id="scrolly">
	{#key currentFigureComponent}
	<figure in:blur={{ delay: 500, duration: 1000 }} out:blur={{ duration: 1000}}>
		<svelte:component this={currentFigureComponent} {...currentFigureComponentProps}/>
	</figure>
	{/key}
	<Scrolly bind:value={scrollyValue}>
		{#each body.scrolly.steps as { id, text }, i}
			{@const active = scrollyValue === i}
			<div data-id={id} class="step" class:active>
				<p class="scrolly-text">{@html text}</p>
			</div>
		{/each}
	</Scrolly>
</section>

<section id="outro">
	<div class="body-content">
		{#each body.outro.content as content} 
			<p class="graf">{@html content.value}</p>
		{/each}
	</div>
</section>


<style lang="scss">
	section {
		position: relative;
	}
	
	.cover {
		height: calc(100svh - 2em);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		.cover-content {
			padding-bottom: 8em;
			text-align: center;
		}
	}

	.hed {
		display: flex;
		gap: 16px;
		font-size: 3rem;

		.hed-front {
			font-family: "IBM Plex Serif";
		}

		.hed-back {
			font-weight: bold;
			// font-family: var(--font-ipa);
			// font-family: "IBM Plex Serif";
			background-color: var(--ipa-yellow);
			padding: 0px 4px;
		}
	}

	.subhed {
		font-size: 20px;
	}

	.byline {
		padding-top: 3em;
		font-family: "Caveat", cursive;
		font-size: 20px;
		color: var(--color-gray-500);
		& a {
			color: var(--color-gray-500);
			text-decoration: 2px underline var(--color-gray-500, blue);
		}
	}

	.body-content {
		width: 40em;
		margin: auto;
		.graf:first-of-type::first-letter {
			font-size: 4em;
			line-height: 1em;
			margin-inline-end: 16px;
			float: inline-start;
			font-family: "IBM Plex Serif";
		}
	}

	.step {
		padding-top: 50svh;
		padding-bottom: 50svh;
		display: flex;
		justify-content: center;
	}

	.step:last-child {
		padding-bottom: 100svh;
	}

	p.scrolly-text {
		padding: 1em;
		border: 2px solid var(--color-fg);
		min-width: min-content;
		// white-space: nowrap;
		z-index: var(--z-overlay);
		position: relative;
		background-color: var(--color-bg);
		box-shadow: 0 4px 16px var(--color-gray-200);
		max-width: 32em;
		text-align: center;
	}

	figure {
		position: sticky;
		top: 0;
		left: 0;
		width: 100%;
		height: 100svh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	#outro {
		padding-bottom: 10em;
	}

	:global(.inline-ipa) {
		font-family: var(--font-ipa);	
		font-weight: bold;
		white-space: nowrap;
	}

	:global(.highlight-pink) {
		background-color: var(--ipa-pink);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
	:global(.highlight-yellow) {
		background-color: var(--ipa-yellow);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}

	:global(.highlight-blue) {
		background-color: var(--ipa-blue);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
	:global(.highlight-purple) {
		background-color: var(--ipa-purple);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
	:global(.highlight-green) {
		background-color: var(--ipa-green);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
	:global(.highlight-orange) {
		background-color: var(--ipa-orange);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
	:global(.highlight-indigo) {
		background-color: var(--ipa-indigo);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
</style>
