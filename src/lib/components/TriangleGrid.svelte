<script>
	import gsap from 'gsap';
	import { onMount } from 'svelte';
	const grid = [14, 30];

	onMount(() => {
		gsap.set('.triangle-grid-item', { opacity: 0, transformOrigin: 'center', color: '#fff' });
		gsap.set('#triangle-grid', { opacity: 1 });

		const tl = gsap.timeline();

		//entrance animation
		tl.to('.triangle-grid-item', {
			keyframes: [
				{ opacity: 0, duration: 0 },
				{
					opacity: 0.4,
					rotate: '+=180',
					color: '#A78BFA',
					duration: 0.6,
					scale: 3,
					stagger: {
						amount: 2,
						grid: grid,
						from: 'center'
					}
				},
				{
					opacity: 0.2,
					rotate: '+=180',
					color: '#fff',
					duration: 0.6,
					scale: 1,
					delay: -2,
					stagger: {
						amount: 3,
						grid: grid,
						from: 'center'
					}
				}
			]
		});

		// repeating animation after entrance

		tl.to('.triangle-grid-item', {
			delay: 11,
			repeat: -1,
			repeatDelay: 11,
			keyframes: [
				{
					opacity: 0.4,
					rotate: '+=180',
					color: '#A78BFA',
					duration: 0.6,
					scale: 3,
					stagger: {
						amount: 2,
						grid: grid,
						from: 'center'
					}
				},
				{
					opacity: 0.2,
					rotate: '+=180',
					color: '#fff',
					duration: 0.6,
					scale: 1,
					delay: -2,
					stagger: {
						amount: 3,
						grid: grid,
						from: 'center'
					}
				}
			]
		});
	});
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	fill="none"
	viewBox="0 0 935 425"
	class="absolute inset-0 -left-2 -top-14 -z-10"
	id="triangle-grid"
	opacity={0}
	style="mask-image:linear-gradient(180deg, black, transparent);"
>
	<g class="triangle-grid-group">
		{#each Array(grid[0]) as _, i}
			{#each Array(grid[1]) as __, j}
				<path
					fill="currentColor"
					opacity=".2"
					class="triangle-grid-item"
					d={`M${j * 32 + 5},${i * 32 + 10}l-3.75,2.165l0,-4.33l3.75,2.165z`}
				/>
			{/each}
		{/each}
	</g>
</svg>
