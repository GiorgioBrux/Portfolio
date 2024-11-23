<script lang="ts">
    export let project: { bgImage: string };
  </script>
  
  <pre class="whitespace-pre text-center font-mono text-white">{project.bgImage}</pre>
  
  <style>
    pre {
      /* Preserve ASCII art formatting */
      line-height: 1.2;
      letter-spacing: 0;
    }
  </style>