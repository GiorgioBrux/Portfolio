<script lang="ts">
    import { onMount } from 'svelte';
    export let project: { bgImage: string };
    import { createNoise2D } from 'simplex-noise';
  
    let blocks: { color: string }[][] = [];
    const GRID_SIZE = 8;
    const GRID_WIDTH = GRID_SIZE + 1; // Extra column for smooth scrolling
    let scrollOffset = 0;
  
    // Brighter, more vibrant color palette
    const blockTypes = {
      grass: '#4CAF50',  // Minecraft grass green
      dirt: '#8D6E63',   // Minecraft dirt brown
      stone: '#B0BEC5',  // Minecraft stone gray
      deepstone: '#78909C' // Darker stone for depth
    };
  
    function getBlockType(noiseValue: number) {
      if (noiseValue > 0.5) return blockTypes.grass;
      if (noiseValue > 0.2) return blockTypes.dirt;
      if (noiseValue > -0.2) return blockTypes.stone;
      return blockTypes.deepstone;
    }
  
    function generateColumn(x: number) {
      const noise2D = createNoise2D();
      const scale = 0.3;
      return Array(GRID_SIZE).fill(null).map((_, y) => {
        const noiseValue = noise2D((x + scrollOffset) * scale, y * scale);
        return {
          color: getBlockType(noiseValue)
        };
      });
    }
  
    function updateBlocks() {
      blocks = Array(GRID_WIDTH).fill(null).map((_, x) => generateColumn(x));
    }
  
    onMount(() => {
      updateBlocks();
      
      // Animate scrolling
      const interval = setInterval(() => {
        scrollOffset += 1;
        updateBlocks();
      }, 2000); // Adjust speed by changing this value
  
      return () => clearInterval(interval);
    });
  </script>
  
  <div class="absolute inset-0 flex items-center justify-center overflow-hidden">
    <!-- Background Grid - Flat Minecraft Blocks -->
    <div class="absolute inset-0 flex items-center justify-center p-16">
      <div 
        class="grid aspect-square w-full max-w-[24rem] gap-1"
        style="grid-template-columns: repeat({GRID_WIDTH}, minmax(0, 1fr))"
      >
        {#each blocks as column, x}
          {#each column as block, y}
            <div 
              class="relative aspect-square w-full transform transition-transform duration-300 rounded-sm hover:-translate-y-1"
              style="background-color: {block?.color};"
            />
          {/each}
        {/each}
      </div>
    </div>
  
  </div>
  
  <style>
    @keyframes slideLeft {
      from {
        transform: translateX(0);
      }
      to {
        transform: translateX(-100%);
      }
    }
  </style>