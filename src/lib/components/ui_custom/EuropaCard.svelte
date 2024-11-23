<script lang="ts">
	import { loadSlim } from '@tsparticles/slim';
	import Particles, { particlesInit } from '@tsparticles/svelte';
	import { onMount } from 'svelte';
    
    let { project, data } : { project: { bgImage: string }, data: { snowConfig: any } } = $props();

    let ParticlesComponent = $state(null as typeof Particles | null);

	onMount(async () => {
		const module = await import('@tsparticles/svelte');
		ParticlesComponent = module.default;
	});

	void particlesInit(async (engine) => {
		await loadSlim(engine);
	});
  </script>
  
  <div class="z-15 absolute inset-0 flex items-center justify-center overflow-hidden">
    {#if ParticlesComponent}
      <ParticlesComponent
        id="snow-particles"
        options={data.snowConfig}
        class="absolute inset-0 h-full w-full p-16"
      />
    {/if}
  
    <!-- Cracks in the Ice Background -->
    <div class="absolute inset-0 grid grid-cols-12 grid-rows-12 gap-0.5 p-16 opacity-20">
      {#each Array(144) as _, i}
        <div
          class="h-full w-full bg-white/10"
          style="clip-path: polygon({Math.random() * 100}% 0, 100% {Math.random() * 100}%, {Math.random() * 100}% 100%, 0 {Math.random() * 100}%);"
        ></div>
      {/each}
    </div>
  
    <!-- Center Content -->
    <div class="relative z-10 flex flex-col items-center">
      <div class="pixel-art mb-2 text-6xl">{project.bgImage}</div>
      <!-- Icy Surface -->
      <div class="relative w-48">
        <div class="h-4 w-full rounded-sm bg-white/30 blur-sm"></div>
        <div class="absolute top-0 h-2 w-full rounded-sm bg-white/40"></div>
      </div>
    </div>
  
    <!-- Space/Ice Gradient -->
    <div class="bg-gradient-radial absolute inset-0 from-transparent via-blue-900/40 to-blue-900/95"></div>
  </div>
  
  <style>
    .pixel-art {
      image-rendering: pixelated;
      image-rendering: crisp-edges;
    }
  
    @keyframes float {
      0%, 100% {
        transform: translateY(0);
      }
      50% {
        transform: translateY(-5px);
      }
	}
</style>
