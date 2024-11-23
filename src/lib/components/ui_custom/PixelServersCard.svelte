<script lang="ts">
    import { onMount } from 'svelte';
    import * as THREE from 'three';
    import { createNoise2D } from 'simplex-noise';

    let container: HTMLElement;
    let scene: THREE.Scene;
    let camera: THREE.PerspectiveCamera;
    let renderer: THREE.WebGLRenderer;
    let island: THREE.Group;

    let rotationSpeed = 0.003; // Slower default speed
    let isHovered = false;

    // Minecraft-like color palette with some aesthetic tweaks
    const blockColors = {
        grass_top: '#55dd55',
        grass_side: '#7fb363',
        dirt: '#866043',
        stone: '#888888',
        bedrock: '#444444',
        water: '#3333ff'
    };

    function createBlock(x: number, y: number, z: number, color: string) {
        const geometry = new THREE.BoxGeometry(1, 1, 1);
        const material = new THREE.MeshPhongMaterial({ 
            color,
            shininess: 0, // Minecraft-style flat shading
        });
        const cube = new THREE.Mesh(geometry, material);
        cube.position.set(x, y, z);
        return cube;
    }

    function generateIsland() {
        const island = new THREE.Group();
        // Use current hour as seed (changes every hour)
        const hourlyTimestamp = Math.floor(Date.now() / (1000 * 60 * 60));
        
        const noise2D = createNoise2D(() => hourlyTimestamp);
        const SIZE = 12;
        
        // Generate blocks centered around (0,0,0)
        for (let x = -SIZE/2; x < SIZE/2; x++) {
            for (let z = -SIZE/2; z < SIZE/2; z++) {
                // Create circular island shape
                const distanceFromCenter = Math.sqrt(
                    Math.pow(x, 2) + 
                    Math.pow(z, 2)
                );
                
                if (distanceFromCenter < SIZE/2) {
                    const height = Math.floor(
                        (noise2D(x * 0.2 + hourlyTimestamp * 0.1, z * 0.2 + hourlyTimestamp * 0.1) + 1) * 3 + 
                        Math.max(0, (SIZE/2 - distanceFromCenter))
                    );

                    for (let y = 0; y < height; y++) {
                        let color;
                        if (y === height - 1) {
                            color = blockColors.grass_top;
                        } else if (y === 0) {
                            color = blockColors.bedrock;
                        } else if (y > height - 3) {
                            color = blockColors.dirt;
                        } else {
                            color = blockColors.stone;
                        }
                        island.add(createBlock(x, y - height/2, z, color));
                    }
                }
            }
        }
        
        return island; // No need to offset position anymore
    }

    function init() {
        scene = new THREE.Scene();
        scene.background = null;

        camera = new THREE.PerspectiveCamera(
            50,
            container.clientWidth / container.clientHeight,
            0.1,
            1000
        );

        renderer = new THREE.WebGLRenderer({ 
            antialias: true,
            alpha: true
        });
        renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(renderer.domElement);

        // Lighting
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
        scene.add(ambientLight);
        
        const mainLight = new THREE.DirectionalLight(0xffffff, 0.8);
        mainLight.position.set(10, 20, 10);
        scene.add(mainLight);

        // Create and add island
        island = generateIsland();
        scene.add(island);

        // Position camera to look directly at the center
        camera.position.set(15, 8, 15);
        camera.lookAt(0, 0, 0);

        animate();
    }

    function animate() {
        requestAnimationFrame(animate);
        
        if (island) {
            // Smoothly interpolate between rotation speeds
            rotationSpeed = isHovered ? 
                rotationSpeed * 0.95 + (0.008 * 0.05) :  // Target faster speed when hovered
                rotationSpeed * 0.95 + (0.003 * 0.05);   // Target slower speed when not hovered
            
            island.rotation.y += rotationSpeed;
        }

        renderer.render(scene, camera);
    }

    function handleMouseEnter() {
        isHovered = true;
    }

    function handleMouseLeave() {
        isHovered = false;
    }

    onMount(() => {
        init();
        return () => renderer?.dispose();
    });
</script>

<div 
    class="absolute inset-0 flex items-center justify-center overflow-hidden" 
    bind:this={container}
    on:mouseenter={handleMouseEnter}
    on:mouseleave={handleMouseLeave}
>
</div>