<script>
	import { getContext, onMount } from "svelte";
	import { browser } from "$app/environment";
	import { blur } from 'svelte/transition';
	import Scrolly from "$components/helpers/Scrolly.svelte";
	import Cat from "$components/Cat.svelte";
	import Pig from "$components/Pig.svelte";
	import Duck from "$components/Duck.svelte";
	import IPA from "$components/IPA.svelte";
	import InlineAudio from "$components/InlineAudio.svelte";
	import Legend from "$components/Legend.svelte";

	const copy = getContext("copy");
	let body = copy.body;
	let hedArr = body.intro.hed.split(" ");

	// scrolly variables
	let scrollyValue;
	let components = {cat: Cat, pig: Pig, duck: Duck};
	let currentFigureComponent;
	let currentFigureId;
	let currentFigureComponentProps;
	let scrollyActiveObj;

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
	
	onMount(() => {
		setTimeout(() => {scrollyValue = 0}, 100);
	});
</script>

<section id="intro">
	<div class="cover-overlay">
		<div class="col">
			<img src="assets/cat-face-logo.png" class="background-img img-1" alt="cat doodle"/>
			<img src="assets/duck-2-purple.png" class="background-img img-2" alt="duck doodle"/>
			<img src="assets/pig-face-logo.png" class="background-img img-3" alt="pig doodle"/>
			<img src="assets/cat-2-orange.png" class="background-img img-1" alt="cat doodle"/>
			<img src="assets/duck-face-2.png" class="background-img img-2" alt="duck doodle"/>
			<img src="assets/pig-2-purple.png" class="background-img img-3" alt="pig doodle"/>
		</div>
		<div class="col">
			<img src="assets/cat-2-pink.png" class="background-img img-3" alt="cat doodle"/>
			<img src="assets/duck-face-2.png" class="background-img img-1" alt="duck doodle"/>
			<img src="assets/pig-2-orange.png" class="background-img img-2" alt="pig doodle"/>
			<img src="assets/cat-face-2.png" class="background-img img-3" alt="cat doodle"/>
			<img src="assets/duck-2-pink.png" class="background-img img-1" alt="duck doodle"/>
			<img src="assets/pig-face-2.png" class="background-img img-2" alt="pig doodle"/>
		</div>
	</div>
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
	<div class="body-content" style="padding-bottom: 8em">
		{#each body.intro.content as content}
		 	{#if content.type === "text"}
			<span class="graf">{@html content.value}</span>
			{:else if content.type === "ipaCatExample"}
				<div style="padding: 1.5em"><IPA ipa="miaw" word="meow" lang="english" ipaScale=2 audioSrc="cat-english"/></div>
			{:else if content.type === "inlineAudio"}
				<InlineAudio audioSrc={content.value.audioSrc} marginRight={content.value?.marginRight}>{@html content.value.slot}</InlineAudio>
			{/if}
		{/each}
	</div>
</section>

<section id="scrolly">
	{#key currentFigureId}
	<div class="scrolly-overlay-container" in:blur={{ delay: 200, duration: 800 }} out:blur={{ duration: 800 }}>
		{#if scrollyValue && currentFigureComponentProps?.display !== "cover"}
		<div class="scrolly-overlay" in:blur={{ delay: 400, duration: 800 }} out:blur={{ duration: 800 }}>
			<div class="animal-logo">
				<img src={`assets/${currentFigureId}-face-logo.png`} width=75 height=75 alt={`${currentFigureId} face doodle`}/>
				<p class="scrolly-hed">{currentFigureId[0].toUpperCase() + currentFigureId.slice(1)}</p>
			</div>
			<div class="legend-container">
				<Legend phoneGroups={copy.phoneGroups[currentFigureId]} pageColors={currentFigureComponentProps?.pageColors} activeObj={scrollyActiveObj}/>
			</div>
		</div>
		{/if}
	</div>
	{/key}
	{#key currentFigureComponentProps}
	<figure 
		in:blur={{ delay: 350, duration: 700 }} out:blur={{ duration: 700}}
		class={currentFigureComponentProps?.display === "cover" && `${currentFigureId}-background`}>
		<svelte:component this={currentFigureComponent} {...currentFigureComponentProps}/>
	</figure>
	{/key}
	<Scrolly bind:value={scrollyValue}>
		{#each body.scrolly.steps as { id, text }, i}
			{@const active = scrollyValue === i}
			<div data-id={id} class="step" class:active>
				{#if text}
					{#if typeof text === 'string'}
						<p class="scrolly-text">{@html text}</p>
					{:else}
					<p class="scrolly-text">
						{#each text as textStr}
							{#if textStr.type === "inlineAudio"}
								<InlineAudio audioSrc={textStr.value.audioSrc} marginRight={textStr.value?.marginRight}>{@html textStr.value.slot}</InlineAudio>
							{:else if textStr.type === "inlinePhoneGroup"}
								<span 
									class="highlight-{textStr.value.color}"
									on:mouseenter={() => {scrollyActiveObj = copy.phoneGroups[currentFigureId].find((d) => d.color === textStr.value.color)}}
									on:mouseleave={() => {scrollyActiveObj = null}}
									style="display: inline-block;cursor: pointer;{textStr.value.marginLeft && "margin-left: 4px;"} {textStr.value.marginRight && "margin-right: 4px;"}"
									>
									<b>{textStr.value.groupName}</b>
								</span>
							{:else}
								<span>{@html textStr.value}</span>
							{/if}
						{/each}
					</p>
					{/if}
				{/if}
			</div>
		{/each}
	</Scrolly>
</section>

<section id="outro">
	<div class="body-content">
		{#each body.outro.content as content}
		 	{#if content.type === "text"}
			<span class="graf">{@html content.value}</span>
			{:else if content.type === "inlineAudio"}
				<InlineAudio audioSrc={content.value.audioSrc} marginRight={content.value?.marginRight}>{@html content.value.slot}</InlineAudio>
			{/if}
		{/each}
	</div>
</section>

<section id="sources">
	<div class="body-content">
		<h3>Sources</h3>
		<ol>
			{#each body.sources as source} 
				<li><a href={source.link}>{source.name}</a>  ({source.source})</li>
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
		max-width: 100%;
		overflow: hidden;

		.cover-content {
			padding-bottom: 8em;
			text-align: center;
		}
	}

	.hed {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		column-gap: 16px;
		font-size: 3rem;

		.hed-front {
			font-family: "IBM Plex Serif";
			margin: 0;
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
		padding: 0 2em;
	}

	.byline {
		padding-top: 3em;
		font-family: "Caveat", cursive;
		font-size: 24px;
		color: var(--color-gray-600);
		& a {
			color: var(--color-gray-600);
			text-decoration: 2px underline var(--color-gray-600, blue);
		}
	}

	.body-content {
		max-width: 40em;
		padding: 0 2em;
		margin: auto;
		font-size: 18px;
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

	.scrolly-text {
		padding: 1em;
		font-size: 18px;
		margin: 16px;
		border: 2px solid var(--color-gray-300);
		min-width: min-content;
		// white-space: nowrap;
		z-index: var(--z-overlay);
		position: relative;
		background-color: var(--color-bg);
		box-shadow: 0 4px 16px rgba(135, 135, 135, 0.3);
		max-width: 32em;
		display: inline-block;
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
		
		&.cat-background {
			background-color: rgba(107, 196, 255, 0.2);
		}

		&.pig-background {
			background-color: rgba(243, 54, 120, 0.2);
		}

		&.duck-background {
			background-color: rgba(255, 159, 81, 0.2);
		}
	}

	#sources {
		padding-bottom: 10em;
	}


	.cover-overlay {
		position: absolute;
		width: 100%;
		display: flex;
		pointer-events: none;
		justify-content: space-between;
		overflow: hidden;
		
		.col {
			width: 36svh;
		}

		.background-img {
			width: 15svh;
			height: 15svh;
			position: relative;
		}
		.img-1 {
			transform: rotate(15deg);
			left: 20.5svh;
		}
		.img-2 {
			transform: rotate(-10deg);
			left: 2svh;
		}
		.img-3 {
			transform: rotate(25deg);
			left: 12svh;
		}
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
		align-items: start;

		@media screen and (max-width: 768px) {
			.g-ipa-chart-subtitle {
				display: none;
			}
		}

		@media screen and (max-width: 1125px) {

			.legend-container {
				display: none;
			}
		}
	}

	.animal-logo {
		display: flex;
		gap: 0.5em;
		align-items: center;

		@media screen and (max-width: 768px) {
			img {
				width: 50px;
				height: 50px;
			} 
			.scrolly-hed {
				font-size: 1.5rem;
			}
		}
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
	:global(.highlight-pink-50) {
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
