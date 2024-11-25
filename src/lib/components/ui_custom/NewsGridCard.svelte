<script lang="ts">
    export let project: { bgImage: string };
  </script>
  
  <div class="absolute inset-0 flex items-center justify-center">
    <!-- Background Grid -->
    <div class="absolute inset-0 grid aspect-square grid-cols-8 grid-rows-8 gap-1 p-16 opacity-40">
      {#each Array(64) as _, i}
        <div
          class="transform rounded-sm bg-white/20 transition-all duration-700 hover:bg-white/40"
          style="animation: pulse {1 + (i % 3)}s ease-in-out infinite {i * 0.1}s"
        ></div>
      {/each}
    </div>
  
    <!-- Center Content -->
    <div class="relative z-10 flex flex-col items-center">
      <div class="mb-2 text-8xl">{project.bgImage}</div>
      <div class="grid grid-cols-4 gap-0.5 opacity-60">
        {#each Array(8) as _}
          <div class="h-0.5 animate-pulse bg-white/40"></div>
        {/each}
      </div>
    </div>
  
    <!-- Radial Gradient Overlay -->
    <div class="bg-gradient-radial absolute inset-0 from-transparent via-emerald-700/20 to-emerald-700/90"></div>
  </div>