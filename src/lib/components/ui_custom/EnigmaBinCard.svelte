<script lang="ts">
    import { onMount } from 'svelte';
    import { User } from 'lucide-svelte';

    let canvas: HTMLCanvasElement;
    let ctx: CanvasRenderingContext2D;
    
    const chars = '01アイウエオカキクケコサシスセソタチツテトナニヌネノ♟︎☯︎✧∰⚡︎⌬⌁⌨︎'.split('');
    
    type Drop = {
        x: number;
        y: number;
        char: string;
        speed: number;
        opacity: number;
    };

    let drops: Drop[] = [];
    let animationFrame: number;

    function initCanvas() {
        const dpr = window.devicePixelRatio || 1;
        const rect = canvas.getBoundingClientRect();
        
        canvas.width = rect.width * dpr;
        canvas.height = rect.height * dpr;
        ctx.scale(dpr, dpr);
        canvas.style.width = `${rect.width}px`;
        canvas.style.height = `${rect.height}px`;
        
        drops = Array(100).fill(null).map(() => ({
            x: rect.width * 0.2,
            y: rect.height / 2 + (Math.random() * 80 - 40),
            char: chars[Math.floor(Math.random() * chars.length)],
            speed: 0.5 + Math.random() * 2,
            opacity: Math.random() * 0.7 + 0.3
        }));
    }

    function draw() {
        const rect = canvas.getBoundingClientRect();
        ctx.clearRect(0, 0, rect.width, rect.height);

        // Draw glowing data stream between users
        const centerY = rect.height / 2;
        ctx.beginPath();
        ctx.strokeStyle = 'rgba(0, 255, 0, 0.3)';
        ctx.lineWidth = 2;
        ctx.moveTo(rect.width * 0.2, centerY);
        ctx.lineTo(rect.width * 0.8, centerY);
        ctx.stroke();
        
        ctx.shadowBlur = 15;
        ctx.shadowColor = 'rgba(0, 255, 0, 0.5)';
        ctx.stroke();
        
        // Draw flowing characters with glow
        drops.forEach(drop => {
            ctx.shadowBlur = 5;
            ctx.shadowColor = 'rgba(0, 255, 0, 0.5)';
            ctx.fillStyle = `rgba(0, 255, 0, ${drop.opacity})`;
            ctx.font = '14px monospace';
            ctx.fillText(drop.char, drop.x, drop.y);

            drop.x += drop.speed;
            if (drop.x > rect.width * 0.8) {
                drop.x = rect.width * 0.2;
                drop.y = rect.height / 2 + (Math.random() * 80 - 40);
                drop.char = chars[Math.floor(Math.random() * chars.length)];
            }
        });

        ctx.shadowBlur = 0;
        animationFrame = requestAnimationFrame(draw);
    }

    onMount(() => {
        ctx = canvas.getContext('2d')!;
        initCanvas();
        draw();

        const resizeObserver = new ResizeObserver(() => {
            initCanvas();
        });
        resizeObserver.observe(canvas);

        return () => {
            cancelAnimationFrame(animationFrame);
            resizeObserver.disconnect();
        };
    });
</script>

<div class="relative h-full w-full p-16">
    <!-- Static Binary Pattern -->
    <div class="absolute inset-0 overflow-hidden mt-16 opacity-20">
        {#each Array(30) as _, i}
            <div
                class="absolute font-mono text-sm text-green-400"
                style="
                    left: {(i * 3.5)}%;
                    top: -{20 + Math.random() * 180}%;
                "
            >
                {#each Array(40) as _}
                    <div>
                        {Math.round(Math.random())}
                    </div>
                {/each}
            </div>
        {/each}
    </div>

    <canvas 
        bind:this={canvas} 
        class="absolute inset-0 h-full w-full"
    />
    
    <!-- User icons -->
    <div class="absolute inset-0 flex items-center justify-between px-12">
        <div class="rounded-full p-4">
            <User class="h-12 w-12 text-green-400" />
        </div>
        <div class="rounded-full p-4">
            <User class="h-12 w-12 text-green-400" />
        </div>
    </div>

    <!-- Radial Gradient Overlay -->
    <div class="bg-gradient-radial absolute inset-0 from-transparent via-blue-900/10 to-blue-900/80"></div>
</div>

<style>
    @keyframes fade-in-out {
        0%, 100% {
            opacity: 0;
        }
        50% {
            opacity: 1;
        }
    }
</style>
