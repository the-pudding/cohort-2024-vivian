<script>
	import volumeIcon from "$svg/volume-icon.svg";

	export let ipa;
	export let word = undefined;
	export let lang;
	export let colors = undefined;
	export let audioSrc = undefined;
	export let ipaScale = 1;
	export let hoverGroupColor = undefined;
	export let showAudioIcon = true;
	export let ipaColumnDisplay = false;

	let letters = ipa.split(/(?<![ˈ͡])(?<!r(?=ʲ))(?![̌ːɲ\d])/);

	function playIPAAudio(audioSrc) {
		const audio = new Audio(audioSrc);
		audio.play();
	}
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="ono-container {audioSrc && 'clickable'} {ipaScale > 1 &&
		`scale-${ipaScale}`} {!colors && 'no-color'} {ipaColumnDisplay &&
		'ipa-column-display'}"
	on:click={() => audioSrc && playIPAAudio(`audio/${audioSrc}.mp3`)}
	on:keydown={() => audioSrc && playIPAAudio(`audio/${audioSrc}.mp3`)}
>
	{#if lang && word}<div class="word {lang}">{word}</div>{/if}
	<div class="ipa-container" style={`transform:scale(${ipaScale})`}>
		<div style={hoverGroupColor && "opacity: 0.2; transition: 0.3s"}>[</div>
		{#each letters as letter}
			<div
				class="ipa-letter {colors && colors[letter]} {hoverGroupColor &&
				(!colors[letter]?.includes(hoverGroupColor) ||
					colors[letter]?.includes(hoverGroupColor + '-'))
					? 'disabled'
					: ''}"
			>
				{letter.replace(/[0-9]/g, "")}
			</div>
		{/each}
		<div style={hoverGroupColor && "opacity: 0.2; transition: 0.3s"}>]</div>
	</div>
	<div class="lang-container">
		{#if lang}<div class="lang">{lang.toUpperCase()}</div>{/if}
		{#if showAudioIcon}
			{@html volumeIcon}
		{/if}
	</div>
</div>

<style lang="scss">
	.ono-container {
		display: grid;
		justify-items: center;
		gap: 0.4em;
		margin: 1.25em;

		&.clickable {
			cursor: pointer;
		}

		&.ipa-column-display {
			grid-template-columns: 1fr 1.5fr;
			justify-items: start;
			margin: 0;

			.ipa-container {
				order: 1;
			}
		}

		&.scale-2 {
			gap: 1.5em;

			&.no-color {
				gap: 1em;
				.ipa-container {
					gap: 0.5px;
				}
			}
		}

		&.scale-2\.5 {
			gap: 2em;
		}
		&.scale-3 {
			gap: 2.5em;
		}
		&.scale-1\.25 {
			gap: 0.6em;
		}

		@media screen and (max-width: 800px) {
			transform: scale(0.8);
			margin: 0;
		}
	}

	.ipa-container {
		display: flex;
		align-items: center;
		gap: 1.5px;
		font-family: var(--font-ipa);
		font-weight: bold;
		font-size: 28px;
	}
	.ipa-letter {
		height: 1.15em;
		line-height: 1.05;
		transition: 0.3s;

		&.disabled {
			opacity: 0.2;
		}
		&.pink {
			background-color: var(--ipa-pink);
		}
		&.orange {
			background-color: var(--ipa-orange);
		}
		&.yellow {
			background-color: var(--ipa-yellow);
		}
		&.green {
			background-color: var(--ipa-green);
		}
		&.blue {
			background-color: var(--ipa-blue);
		}
		&.indigo {
			background-color: var(--ipa-indigo);
		}
		&.purple {
			background-color: var(--ipa-purple);
		}
		&.brown {
			background-color: var(--ipa-brown);
		}
		&.seafoam {
			background-color: var(--ipa-seafoam);
		}
		&.stone {
			background-color: var(--ipa-stone);
		}
		&.pink-50 {
			background-color: var(--ipa-pink-50);
		}
		&.purple-50 {
			background-color: var(--ipa-purple-50);
		}
		&.indigo-50 {
			background-color: var(--ipa-indigo-50);
		}
		&.pink75-yellow25 {
			background: linear-gradient(
				to right,
				var(--ipa-pink) 78%,
				var(--ipa-yellow) 22%
			);
		}
		&.pink50-yellow50 {
			background: linear-gradient(
				to right,
				var(--ipa-pink) 50%,
				var(--ipa-yellow) 50%
			);
		}
		&.green75-yellow25 {
			background: linear-gradient(
				to right,
				var(--ipa-green) 78%,
				var(--ipa-yellow) 22%
			);
		}
	}
	.word {
		&.hindi {
			font-family: var(--font-hindi);
		}
		font-size: 15px;
	}
	.lang-container {
		display: flex;
		gap: 6px;
		align-items: center;
	}
	.lang {
		white-space: nowrap;
		color: var(--color-gray-600);
		font-size: 14px;
		line-height: 1;
	}
</style>
