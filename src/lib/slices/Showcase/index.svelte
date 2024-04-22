<script>
	import clsx from 'clsx';
	import gsap from 'gsap';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';
	import { onMount } from 'svelte';

	import Bounded from '$lib/components/Bounded.svelte';
	import ButtonLink from '$lib/components/ButtonLink.svelte';
	import { PrismicImage, PrismicRichText } from '@prismicio/svelte';
	import IconCycle from '~icons/ph/arrows-clockwise';
	import IconGear from '~icons/ph/gear';

	/** @type {import("@prismicio/client").Content.ShowcaseSlice} */
	export let slice;

	const ICONS = {
		Gear: IconGear,
		Cycle: IconCycle
	};

	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);

		gsap.fromTo(
			'.showcase__heading',
			{
				y: 100
			},
			{
				y: 0,
				scrollTrigger: {
					trigger: '.showcase__heading',
					markers: true,
					start: 'top bottom-=40%',
					toggleActions: 'play pause resume reverse'
				}
			}
		);

		gsap.fromTo(
			'.showcase__glow',
			{
				scale: 0.2,
				opacity: 0
			},
			{
				scale: 1,
				opacity: 0.35,
				scrollTrigger: {
					trigger: '.showcase__heading',
					start: 'top bottom-=40%',
					toggleActions: 'play pause resume reverse'
				}
			}
		);
	});
</script>

<Bounded data-slice-type={slice.slice_type} data-slice-variation={slice.variation} class="relative">
	<div
		class="showcase__glow absolute -z-10 aspect-video w-full max-w-2xl rounded-full bg-violet-500 blur-[120px] filter"
	/>
	{#if slice.primary.heading}
		<h2 class="showcase__heading text-balance text-center text-5xl font-medium md:text-7xl">
			<PrismicRichText field={slice.primary.heading} />
		</h2>
	{/if}
	<div
		class="relative mt-16 grid gap-8 rounded-xl border border-violet-50/20 bg-gradient-to-b from-gray-50/15 to-gray-50/5 px-8 py-8 backdrop-blur-sm lg:grid-cols-3 lg:gap-0 lg:py-12"
	>
		<div class="grid-background"></div>
		<div>
			{#if slice.primary.icon}
				<div class="w-fit rounded-lg bg-violet-800 p-4 text-3xl">
					<svelte:component this={ICONS[slice.primary.icon]} />
				</div>
			{/if}
			{#if slice.primary.subheading}
				<h3 class="mt-6 text-3xl font-normal">
					<PrismicRichText field={slice.primary.subheading} />
				</h3>
			{/if}
			{#if slice.primary.body}
				<div class="prose prose-invert mt-4 max-w-xl">
					<PrismicRichText field={slice.primary.body} />
				</div>
			{/if}
			{#if slice.primary.button_link && slice.primary.button_text}
				<ButtonLink field={slice.primary.button_link} class="mt-6">
					{slice.primary.button_text}
				</ButtonLink>
			{/if}
		</div>
		{#if slice.primary.image}
			<PrismicImage
				field={slice.primary.image}
				class={clsx(
					'pt-0 opacity-90 shadow-2xl lg:col-span-2',
					slice.variation === 'reverse'
						? 'lg:order-1 lg:translate-x-[15%]'
						: 'lg:-order-1 lg:-translate-x-[15%]'
				)}
			/>
		{/if}
	</div>
</Bounded>

<style>
	.grid-background {
		background-image: url('/assets/grid-pattern.png');
		position: absolute;
		inset: 0;
		z-index: -1;
		background-position: center;
		opacity: 0.15;
		mask-image: radial-gradient(circle at 60% 50%, black 10%, transparent 40%);
	}
</style>
