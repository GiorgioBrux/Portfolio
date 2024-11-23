<script lang="ts">
	import { Card, CardContent, CardFooter, CardHeader, CardTitle } from '$lib/components/ui/card';
	import { Button } from '$lib/components/ui/button';
	import { Github, Star, Sun, Moon, MapPin, ExternalLink } from 'lucide-svelte';
	import NewsGridCard from '$lib/components/ui_custom/NewsGridCard.svelte';
	import EuropaCard from '$lib/components/ui_custom/EuropaCard.svelte';
	import PixelServersCard from '$lib/components/ui_custom/PixelServersCard.svelte';
	import AsciiArtCard from '$lib/components/ui_custom/AsciiArtCard.svelte';
	import { onMount } from 'svelte';

	let { data } = $props();

	let personalInfo = $state({
		name: 'Giorgio B.',
		title: 'Software Developer',
		description:
			"I'm a Computer Science student passionate about software development, currently working on my Bachelor's thesis. I try to build cool stuff, keeping user privacy and security in mind.",
		calltoaction:
			"If you are reading this from the EU, please help us stop Chat Control, the EU's proposed warrantless mass surveillance program.",
		location: 'Catania, Italy'
	});

	let projects = $state([
		{
			title: 'PixelServers',
			description: 'WIP Minecraft server hosting platform.',
			productlink: 'https://pixel-servers.vercel.app/',
			bgImage: '💻',
			bgColor: 'from-purple-600 to-pink-600',
			isPixelServers: true
		},
		{
			title: 'NewsGrid',
			description: "AI-powered news aggregator, alternative front-end for Kagi's Kite.",
			link: 'https://github.com/GiorgioBrux/newsgrid',
			productlink: 'https://newsgrid.reizouko.eu',
			bgImage: '📰',
			stars: 0,
			bgColor: 'from-emerald-600 to-emerald-800',
			isNewsGrid: true
		},
		{
			title: 'Europa',
			description: "2D platformer game about a penguin exploring Jupiter's icy moon Europa.",
			link: 'https://github.com/giorgiobrullo/europa',
			productlink: 'https://europa.giorgiobrux.eu',
			bgImage: '🐧',
			stars: 0,
			bgColor: 'from-blue-900 to-indigo-900',
			isEuropa: true
		},
		{
			title: 'Nitro Sniper',
			description:
				'A functional self-bot for Discord that automatically tries to redeem Nitro codes.',
			link: 'https://github.com/GiorgioBrux/nitro-sniper-enhanced',
			bgImage: '/images/nitrosniper.png',
			stars: 43,
			bgColor: 'from-blue-600 to-blue-800'
		},
		{
			title: 'Advent of Code',
			description: 'Mixed-language solutions for the 2023+ challenges.',
			link: 'https://github.com/giorgiobrullo/aoc-kotlin',
			bgImage: `★
/\\
/o \\
/  o \\
/o  *  \\
/*  o  * \\
/  *  o  * \\
/*  o  *  o  \\
/o  *  o  *  o \\
/_______________\\
||`,
			stars: 0,
			bgColor: 'from-orange-600 to-orange-800',
			isAsciiArt: true
		},

	]);

	let isDarkMode = $state(false);

	onMount(() => {
		// Check system preference on mount
		const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
		isDarkMode = prefersDark;

		// Listen for system preference changes
		window.matchMedia('(prefers-color-scheme: dark)')
			.addEventListener('change', (e) => {
				isDarkMode = e.matches;
			});
	});

	function toggleDarkMode() {
		isDarkMode = !isDarkMode;
	}
</script>

<div
	class="{isDarkMode
		? 'bg-gradient-to-br from-gray-900 to-gray-800 text-white'
		: 'bg-white text-gray-800'} min-h-screen transition-colors duration-300"
>
	<div class="container mx-auto max-w-6xl px-4 py-12">
		<div class="mb-4 flex items-center justify-end gap-2">
			{#if !isDarkMode}
				<span class="text-sm font-medium italic">Flashbanged? -&gt;</span>
			{/if}
			<Button variant="outline" size="icon" onclick={toggleDarkMode} class="rounded-full">
				{#if isDarkMode}
					<Sun class="h-5 w-5" />
				{:else}
					<Moon class="h-5 w-5" />
				{/if}
			</Button>
		</div>

		<Card class="mb-12 bg-gradient-to-r from-purple-600 to-pink-600 text-white shadow-xl {isDarkMode
			? 'border-gray-700 bg-gray-800'
			: 'border-gray-200 bg-white'}">
			<CardHeader>
				<div class="flex items-center space-x-4">
					<div class="text-5xl">👋</div>
					<div>
						<CardTitle class="text-4xl font-bold">I'm {personalInfo.name}</CardTitle>
						<p class="text-xl text-white/80">{personalInfo.title}</p>
						<a
							href="https://www.google.com/maps/place/Catania,+Italy"
							target="_blank"
							rel="noopener noreferrer"
							class="flex items-center gap-2 text-xl text-white/80 transition-colors hover:text-white"
						>
							<MapPin />
							{personalInfo.location}
						</a>
					</div>
				</div>
			</CardHeader>
			<CardContent>
				<p class="text-lg">{personalInfo.description}</p>
				<a
					href="https://www.patrick-breyer.de/en/take-action-to-stop-chat-control-now/"
					target="_blank"
					rel="noopener noreferrer"
					class="mt-4 flex items-center gap-2 text-lg text-white/80 transition-colors hover:text-white"
				>
					{personalInfo.calltoaction}
				</a>
			</CardContent>
			<CardFooter class="flex justify-start space-x-4">
				<Button
					variant="secondary"
					size="icon"
					class="rounded-full hover:bg-white hover:text-purple-600"
					href="https://github.com/GiorgioBrux"
				>
					<Github class="h-5 w-5" />
				</Button>
				<Button
					variant="secondary"
					size="icon"
					class="rounded-full hover:bg-white hover:text-purple-600"
					href="https://giorgiobrux.bsky.social"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 600 530"
						fill="currentColor"
						class="h-5 w-5"
					>
						<path
							d="M135.72 44.03c66.496 49.921 138.02 151.14 164.28 205.46 26.262-54.316 97.782-155.54 164.28-205.46 47.98-36.021 125.72-63.892 125.72 24.795 0 17.712-10.155 148.79-16.111 170.07-20.703 73.984-96.144 92.854-163.25 81.433 117.3 19.964 147.14 86.092 82.697 152.22-122.39 125.59-175.91-31.511-189.63-71.766-2.514-7.3797-3.6904-10.832-3.7077-7.8964-0.0174-2.9357-1.1937 0.51669-3.7077 7.8964-13.714 40.255-67.233 197.36-189.63 71.766-64.444-66.128-34.605-132.26 82.697-152.22-67.108 11.421-142.55-7.4491-163.25-81.433-5.9562-21.282-16.111-152.36-16.111-170.07 0-88.687 77.742-60.816 125.72-24.795z"
						/>
					</svg>
				</Button>
			</CardFooter>
		</Card>

		<div class="grid grid-cols-1 gap-8 md:grid-cols-2">
			{#each projects as project}
				<Card
					class="group flex h-full flex-col overflow-hidden transition-all duration-300 hover:shadow-2xl {isDarkMode
						? 'border-gray-700 bg-gray-800'
						: 'border-gray-200 bg-white'}"
				>
					<div class="aspect-square w-full bg-gradient-to-br {project.bgColor} relative p-8">
						<div class="absolute inset-0 flex flex-col justify-between p-6">
							<div>
								<h3 class="text-2xl font-bold text-white">{project.title}</h3>
								<p class="mt-2 text-sm text-gray-100">{project.description}</p>
							</div>	
							{#if project.stars}
								<div class="flex items-center space-x-2">
									<Star class="h-5 w-5 text-yellow-400" />
									<span class="font-semibold text-yellow-400">{project.stars}</span>
								</div>
							{/if}
						</div>

						<div class="relative flex h-full w-full items-center justify-center overflow-hidden">
							{#if project.isAsciiArt}
							<AsciiArtCard {project} />
						  {:else if project.isNewsGrid}
							<NewsGridCard {project} />
						  {:else if project.isEuropa}
							<EuropaCard {project} {data} />
						  {:else if project.isPixelServers}
							<PixelServersCard />
						  {:else}
							<img src={project.bgImage} alt={project.title} />
						  {/if}
						</div>

						{#if project.link}
						<Button
							href={project.link}
							target="_blank"
							rel="noopener noreferrer"
								variant="secondary"
								size="icon"
								class="absolute right-4 top-4 rounded-full opacity-0 transition-opacity duration-300 hover:bg-white hover:text-purple-600 group-hover:opacity-100"
							>
								<Github class="h-5 w-5" />
							</Button>
						{/if}
						{#if project.productlink}
							<Button
								href={project.productlink}
								target="_blank"
								rel="noopener noreferrer"
								variant="secondary"
								size="icon"
								class="absolute {project.link ? 'right-16' : 'right-4'} top-4 rounded-full opacity-0 transition-opacity duration-300 hover:bg-white hover:text-purple-600 group-hover:opacity-100"
							>
								<ExternalLink class="h-5 w-5" />
							</Button>
						{/if}
					</div>
				</Card>
			{/each}
		</div>
		<footer class="mt-12 text-center">
			<a
				href="https://github.com/GiorgioBrux/portfolio"
				target="_blank"
				rel="noopener noreferrer"
				class="inline-flex items-center gap-2 text-sm opacity-60 transition-opacity hover:opacity-100"
			>
				<Github class="h-4 w-4" />
				View source code on GitHub
			</a>
		</footer>
	</div>
</div>

<style>
	.pixel-art {
		image-rendering: pixelated;
		image-rendering: crisp-edges;
	}
</style>
