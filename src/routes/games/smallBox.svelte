<script>
	export let image;

	export let title;
	export let id;

	export let color;

	export let category;

	export let popular;

	import { onMount } from 'svelte';
	import ColorThief from '$lib/components/color-thief.mjs';

	let shine;

	onMount(async () => {
		async function waitUntilImageChange() {
			while (image == 'loading') {
				await new Promise(resolve => setTimeout(resolve, 100));
			}
			const colorThief = new ColorThief();
			const img = new Image();
			img.src = `./game/img/${image}`;
			img.addEventListener('load', function () {
				shine = `rgb(${colorThief.getColor(img)})`;
			});
		}
		await waitUntilImageChange();
		
	});
</script>

{#if popular != undefined}
	<a class="game" id={title} href={'/games/' + id}>
		<div
			class="background-game bg-white dark:bg-zinc-900 shadow-lg hover:bg-gray-200 shadow-gray-200 dark:shadow-orange-300 hover:cursor-pointer w-full h-full mb-5 rounded-xl transition duration-100 hover:scale-[102.5%]"
		>
			<div class="p-5 w-full justify-center flex">
				<img
					loading="lazy"
					src={'/game/img/' + image}
					class="img w-auto max-h-[10rem]"
				/>
			</div>
			<div class="pl-5 pr-5">
				<h1 class="font-bold text-2xl break-all" style="color: {color};">{title}</h1>
			</div>
		</div>
	</a>
{:else}
	<a class="game" id={title} href={'/games/' + id}>
		<div
			class="background-game bg-white dark:bg-zinc-900 hover:bg-gray-200 hover:cursor-pointer hover:shadow-2xl h-full mb-5 rounded-xl transition duration-100 hover:scale-[102.5%]"
			style={`box-shadow: 0 10px 15px -3px ${shine}`}
		>
			<div class="p-5 w-full justify-center flex">
				<img
					loading="lazy"
					src={'/game/img/' + image}
					class="img w-auto max-h-[10rem]"
				/>
			</div>
			<div class="pl-5 pr-5">
				<h1 class="font-bold text-2xl break-words text-black dark:text-white" style="color: {color};">{title}</h1>
			</div>
		</div>
	</a>
{/if}
