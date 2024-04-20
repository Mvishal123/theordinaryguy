<script>
	import { PrismicLink } from '@prismicio/svelte';
	import WordMark from './WordMark.svelte';
	import ButtonLink from './ButtonLink.svelte';

	/** @type {import("@prismicio/client").Content.SettingsDocument}*/
	export let settings;
</script>

<footer>
	<nav
		class="flex flex-col items-center justify-between gap-6 border-t border-x-gray-600 px-8 py-7 md:flex-row"
		aria-label="Main"
	>
		<a href="/">
			<WordMark />
			<span class="sr-only">{settings.data.site_title}</span>
		</a>
		<ul class="flex gap-6">
			{#each settings.data.navigation as item (item.label)}
				{#if item.cta_button}
					<li>
						<ButtonLink>{item.label}</ButtonLink>
					</li>
				{:else}
					<li>
						<PrismicLink field={item.link} class="inline-flex min-h-11 items-center"
							>{item.label}</PrismicLink
						>
					</li>
				{/if}
			{/each}
		</ul>
	</nav>
</footer>
