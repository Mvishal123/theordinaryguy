<script>
	import clsx from 'clsx';
	import { PrismicRichText, PrismicText } from '@prismicio/svelte';
	import gsap from 'gsap';
	import { onMount } from 'svelte';

	import Bounded from '$lib/components/Bounded.svelte';
	import LogoBackground from './LogoBackground.svelte';
	import StylizedLogoMark from './StylizedLogoMark.svelte';
	import background from './integration.jpg';

	import logoNPM from '~icons/fa6-brands/npm';
	import logoFigma from '~icons/fa6-brands/figma';
	import logoGithub from '~icons/fa6-brands/github';
	import logoCloudflare from '~icons/fa6-brands/cloudflare';
	import logoDigitalOcean from '~icons/fa6-brands/digital-ocean';
	import logoFly from '~icons/fa6-brands/fly';

	const icons = {
		npm: logoNPM,
		figma: logoFigma,
		github: logoGithub,
		cloudflare: logoCloudflare,
		digitalocean: logoDigitalOcean,
		fly: logoFly
	};

	/** @type {import("@prismicio/client").Content.IntegrationsSlice} */
	export let slice;

	onMount(() => {
		const tl = gsap.timeline({
			repeat: -1,
			defaults: {
				ease: 'power2.inOut'
			}
		});
	});
</script>

<Bounded
	data-slice-type={slice.slice_type}
	data-slice-variation={slice.variation}
	class="relative overflow-hidden"
>
	<img src={background} alt="background" class="absolute inset-0 h-full w-full object-cover" />
	<LogoBackground />
	<div class="relative">
		<h2
			class="mx-auto max-w-2xl bg-gradient-to-b from-violet-50 to-violet-300 bg-clip-text py-2 text-center text-5xl font-medium text-transparent md:text-7xl"
		>
			<PrismicText field={slice.primary.heading} />
		</h2>

		<div class="mx-auto mt-6 max-w-md text-balance text-center text-gray-300">
			<PrismicRichText field={slice.primary.body} />
		</div>

		<div class="mt-20 flex flex-col items-center md:flex-row">
			{#each slice.items as item, index}
				{#if item.icon}
					{#if Math.floor(slice.items.length) / 2 === index}
						<StylizedLogoMark />
						<div class="signal-line rotate-180"></div>
					{/if}
					<div
						class="pulsing-icon flex aspect-square shrink-0 items-center justify-center rounded-full border border-violet-50/30 bg-violet-50/35 p-3 text-3xl text-violet-100 opacity-40 md:text-5xl"
					>
						<svelte:component this={icons[item.icon]} />
					</div>
					{#if index !== slice.items.length - 1 && item.icon}
						<div
							class={clsx(
								'signal-line',
								index >= Math.floor(slice.items.length / 2) ? 'rotate-180' : 'rotate-0'
							)}
						></div>
					{/if}
				{/if}
			{/each}
		</div>
	</div>
</Bounded>
