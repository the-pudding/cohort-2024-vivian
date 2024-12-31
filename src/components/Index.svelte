<script>
	import { getContext } from "svelte";
	import { browser } from "$app/environment";
	import { blur } from 'svelte/transition';
	import Scrolly from "$components/helpers/Scrolly.svelte";
	import Cat from "$components/Cat.svelte";
	import Pig from "$components/Pig.svelte";
	import Duck from "$components/Duck.svelte";
	import IPA from "$components/IPA.svelte";
	import volumeIcon from "$svg/volume-icon.svg";

	const copy = getContext("copy");
	let body = copy.body;
	let hedArr = body.intro.hed.split(" ");

	// scrolly variables
	let scrollyValue;
	let components = {cat: Cat, pig: Pig, duck: Duck};
	let currentFigureComponent;
	let currentFigureId;
	let currentFigureComponentProps;

	function updateScrolly() {
		if (scrollyValue === undefined) { 

			currentFigureComponent = undefined;
			currentFigureId = undefined;
			currentFigureComponentProps = undefined;

			return; 
		}
		const selector = `.step:nth-of-type(${scrollyValue + 1})`;
		const id = document.querySelector(selector).getAttribute("data-id");
		currentFigureComponent = components[id];
		currentFigureId = id;
		if (currentFigureComponentProps?.display !== body.scrolly.steps[scrollyValue].props?.display) {
			currentFigureComponentProps = body.scrolly.steps[scrollyValue].props;
		}
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
	{#key currentFigureId}
	<div class="scrolly-overlay-container" in:blur={{ delay: 400, duration: 800 }} out:blur={{ duration: 800 }}>
		{#if scrollyValue && currentFigureComponentProps?.display !== "cover"}
		<div class="scrolly-overlay" in:blur={{ delay: 400, duration: 800 }} out:blur={{ duration: 800 }}>
			<div class="animal-logo">
				<img src={`assets/${currentFigureId}-face-logo.png`} width=75 height=75 alt={`${currentFigureId} face doodle`}/>
				<p class="scrolly-hed">{currentFigureId[0].toUpperCase() + currentFigureId.slice(1)}</p>
			</div>
			<div class="g-ipa-chart-subtitle"><p class="ipa-chart-subtitle">Click an element to hear it aloud</p>{@html volumeIcon}</div>
		</div>
		{/if}
	</div>
	{/key}
	{#key currentFigureComponentProps}
	<figure in:blur={{ delay: 400, duration: 800 }} out:blur={{ duration: 800}}>
		<svelte:component this={currentFigureComponent} {...currentFigureComponentProps}/>
	</figure>
	{/key}
	<Scrolly bind:value={scrollyValue}>
		{#each body.scrolly.steps as { id, text }, i}
			{@const active = scrollyValue === i}
			<div data-id={id} class="step" class:active>
				{#if text}<p class="scrolly-text">{@html text}</p>{/if}
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

<section id="sources">
	<div class="body-content">
		<h3>Sources</h3>
		<ol>
			{#each body.sources as source} 
				<li><a href={source.link}>{source.name}</a></li>
			{/each}
		</ol>
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
		border: 2px solid var(--color-gray-300);
		min-width: min-content;
		// white-space: nowrap;
		z-index: var(--z-overlay);
		position: relative;
		background-color: var(--color-bg);
		box-shadow: 0 4px 16px rgba(135, 135, 135, 0.3);
		max-width: 32em;
		text-align: center;
	}

	figure {
		position: sticky;
		top: 20px;
		left: 0;
		width: 100%;
		height: 100svh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	#sources {
		padding-bottom: 10em;
	}

	.scrolly-overlay-container {
		position: fixed;
		top: 0;
		left: 0;
		z-index: 10;
	}

	.scrolly-overlay {
		display: flex;
		width: 100vw;
		padding: 0.5em 1.5em;
		justify-content: space-between;
	}

	.animal-logo {
		display: flex;
		gap: 0.5em;
		align-items: center;
	}

	.scrolly-hed {
		font-size: 1.75rem;
		font-family: "IBM Plex Serif";
		color: var(--color-gray-700);
	}

	.g-ipa-chart-subtitle {
		margin: 0 0 20px 12px;
		display: flex;
		align-items: center;
		gap: 4px;
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
	:global(.highlight-pink50) {
		background-color: var(--ipa-pink-50);
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
	:global(.highlight-stone) {
		background-color: var(--ipa-stone);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
	:global(.highlight-brown) {
		background-color: var(--ipa-brown);
		line-height: 1.1;
		display: inline-block;
		padding: 0 1px;
	}
</style>
