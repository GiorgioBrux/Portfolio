<script lang="ts">
    import type { Snippet } from 'svelte';
	import { Card } from '../ui/card';
	import { Button } from '../ui/button';
	import { Github, ExternalLink } from 'lucide-svelte';
    
    interface Project {
        title: string;
        description: string;
        bgColor: string;
        link?: string;
        productlink?: string;
    }

    interface Props {
        project: Project;
        isDarkMode: boolean;
        stats: Snippet;
        card: Snippet;
    }

    let { project, isDarkMode, stats, card } : Props = $props();
</script>
  
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
        {@render stats()}
      </div>
  
      <div class="relative flex h-full w-full items-center justify-center overflow-hidden">
        {@render card()}
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
