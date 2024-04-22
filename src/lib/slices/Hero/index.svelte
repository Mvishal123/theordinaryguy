<script>
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	import { PrismicImage, PrismicLink, PrismicText } from '@prismicio/svelte';

	import Bounded from '$lib/components/Bounded.svelte';
	import ButtonLink from '$lib/components/ButtonLink.svelte';
	import TriangleGrid from '$lib/components/TriangleGrid.svelte';

	/** @type {import("@prismicio/client").Content.HeroSlice} */
	export let slice;

	onMount(() => {
		const tl = gsap.timeline({ defaults: { ease: 'power2.out' } });

		tl.fromTo('.hero__header', { scale: 1.25 }, { scale: 1, opacity: 1, duration: 1.8 });
		tl.fromTo('.hero__body', { y: 100 }, { y: 0, duration: 1.4, opacity: 1 }, '-=1');
		tl.fromTo('.hero__button', { scale: 1.5 }, { scale: 1, opacity: 1, duration: 1.2 }, '-=1');
		tl.fromTo('.hero__image', { y: 100 }, { y: 0, opacity: 1, duration: 1.4 }, '+=.3');
		tl.fromTo('.hero__imageglow', { scale: 0.5 }, { scale: 1, opacity: 1, duration: 1.4 }, '-=1');

		gsap.to('.hero__imageglow_1', {
			keyframes: [
				{ top: '0%', left: '33.33%', duration: 0 },
				{ top: '33.33%', left: '33.33%', duration: 2 },
				{ top: '33.33%', left: '0%', duration: 3 },
				{ top: '0%', left: '0%', duration: 2 },
				{ top: '0%', left: '33.33%', duration: 3 }
			],
			repeat: -1,
			repeatDelay: 3,

			delay: 5
		});
		gsap.to('.hero__imageglow_2', {
			keyframes: [
				{ top: '33.33%', left: '0%', duration: 0 },
				{ top: '0%', left: '0%', duration: 2 },
				{ top: '0%', left: '33.33%', duration: 3 },
				{ top: '33.33%', left: '33.33%', duration: 2 },
				{ top: '33.33%', left: '0%', duration: 3 }
			],
			repeat: -1,
			repeatDelay: 3,
			delay: 5
		});
	});
</script>

<Bounded data-slice-type={slice.slice_type} data-slice-variation={slice.variation}>
	<div class="relative w-full text-center">
		<TriangleGrid />

		{#if slice.primary.heading}
			<h1
				class="hero__header mx-auto max-w-3xl text-balance text-5xl font-medium opacity-0 md:text-7xl"
			>
				<PrismicText field={slice.primary.heading} />
			</h1>
		{/if}
		{#if slice.primary.body}
			<p class="hero__body mx-auto mt-6 max-w-md text-balance text-gray-300 opacity-0">
				<PrismicText field={slice.primary.body} />
			</p>
		{/if}
		{#if slice.primary.button_link}
			<ButtonLink class="hero__button mt-8 opacity-0">
				<PrismicLink field={slice.primary.button_link} class="hero__button opacity-0"
					>{slice.primary.button_label}</PrismicLink
				>
			</ButtonLink>
		{/if}
		{#if slice.primary.image}
			<div class="glass-container hero__image mt-16 w-fit opacity-0">
				<div
					class="hero__imageglow hero__imageglow_1 absolute left-1/3 top-0 -z-10 h-2/3 w-2/3 bg-violet-700/50 opacity-0 mix-blend-screen blur-3xl md:blur-[120px]"
				/>
				<div
					class="hero__imageglow hero__imageglow_2 absolute bottom-0 left-0 -z-10 h-2/3 w-2/3 bg-orange-600/50 opacity-0 mix-blend-screen blur-3xl md:blur-[120px]"
				/>
				<PrismicImage field={slice.primary.image} class="rounded-xl" />
			</div>
		{/if}
	</div>
</Bounded>
