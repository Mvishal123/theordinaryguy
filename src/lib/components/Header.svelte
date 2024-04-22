<script>
	import { PrismicLink } from '@prismicio/svelte';
	import clsx from 'clsx';
	import ButtonLink from './ButtonLink.svelte';
	import WordMark from './WordMark.svelte';

	import IconClose from '~icons/ph/x-bold';
	import IconMenu from '~icons/ph/list-bold';
	import { asLink } from '@prismicio/client';
	import { page } from '$app/stores';

	/** @type {import("@prismicio/client").Content.SettingsDocument}*/
	export let settings;

	let isOpen = false;
	const toggleOpen = () => {
		isOpen = !isOpen;
	};
	const close = () => {
		isOpen = false;
	};

	/**@param {import("@prismicio/client").LinkField} link*/
	const isActive = (link) => {
		const path = asLink(link);
		return path && $page.url.pathname.includes(path);
	};
</script>

<header>
	<nav class="item-center flex w-full justify-between px-6 py-6 md:px-44" aria-label="Main">
		<div class="flex items-center justify-between max-md:w-full">
			<a href="/" on:click={close} class="z-50">
				<WordMark />
				<span class="sr-only">{settings.data.site_title}</span>
			</a>
			<button
				aria-expanded={isOpen}
				type="button"
				class="block p-2 text-3xl text-white md:hidden"
				on:click={toggleOpen}
			>
				<IconMenu />
			</button>
		</div>

		<!-- Mobile navigation -->
		<div
			class={clsx(
				'fixed inset-0 z-40 flex flex-col items-end bg-gray-950 pr-4 pt-6 transition-transform duration-200 ease-in-out md:hidden',
				isOpen ? 'translate-x-0' : 'translate-x-[100%]'
			)}
		>
			<button
				type="button"
				class="block p-2 text-3xl text-white md:hidden"
				on:click={toggleOpen}
				aria-expanded={isOpen}
			>
				<IconClose />
			</button>
			<ul class="grid justify-items-end gap-8">
				{#each settings.data.navigation as item (item.label)}
					{#if item.cta_button}
						<li>
							<ButtonLink on:click={close} aria-current={isActive(item.link) ? 'page' : undefined}
								>{item.label}</ButtonLink
							>
						</li>
					{:else}
						<li>
							<PrismicLink
								on:click={close}
								field={item.link}
								class=" block min-h-11 px-3 text-3xl first:mt-8"
								aria-current={isActive(item.link) ? 'page' : undefined}>{item.label}</PrismicLink
							>
						</li>
					{/if}
				{/each}
			</ul>
		</div>
		<!-- Desktop navigation -->
		<ul class="hidden gap-6 md:flex">
			{#each settings.data.navigation as item (item.label)}
				{#if item.cta_button}
					<li>
						<ButtonLink aria-current={isActive(item.link) ? 'page' : undefined}
							>{item.label}</ButtonLink
						>
					</li>
				{:else}
					<li>
						<PrismicLink
							field={item.link}
							class="inline-flex min-h-11 items-center"
							aria-current={isActive(item.link) ? 'page' : undefined}>{item.label}</PrismicLink
						>
					</li>
				{/if}
			{/each}
		</ul>
	</nav>
</header>
