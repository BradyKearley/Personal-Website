<script lang="ts">
  import { onMount } from 'svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  import { fly } from 'svelte/transition';
  
  export let skill: string;
  export let level: number; // 0-100
  export let color: string = 'var(--accent-primary)';
  export let icon: string = '⚡';
  export let size: number = 140;
  export let episodes: number = 0; // RL "episodes" for the iteration display
  
  const progress = tweened(0, {
    duration: 1500,
    easing: cubicOut
  });
  
  $: circumference = 2 * Math.PI * 45;
  $: strokeDashoffset = circumference - ($progress / 100) * circumference;
  
  let visible = false;
  
  onMount(() => {
    visible = true;
    setTimeout(() => {
      progress.set(level);
    }, 300);
  });
</script>

<div 
  class="skill-ring"
  style="--size: {size}px; --color: {color}"
  in:fly={{ y: 30, duration: 600 }}
>
  <svg viewBox="0 0 100 100" class="ring-svg">
    <!-- Hour marks (clock-style) -->
    {#each Array(12) as _, i}
      {@const angle = (i * 30 - 90) * (Math.PI / 180)}
      {@const x1 = 50 + 42 * Math.cos(angle)}
      {@const y1 = 50 + 42 * Math.sin(angle)}
      {@const x2 = 50 + 38 * Math.cos(angle)}
      {@const y2 = 50 + 38 * Math.sin(angle)}
      <line 
        {x1} {y1} {x2} {y2}
        class="tick-mark"
        class:main-tick={i % 3 === 0}
      />
    {/each}
    
    <!-- Background ring -->
    <circle
      cx="50"
      cy="50"
      r="45"
      class="ring-bg"
      fill="none"
      stroke-width="8"
    />
    
    <!-- Progress ring -->
    <circle
      cx="50"
      cy="50"
      r="45"
      class="ring-progress"
      fill="none"
      stroke-width="8"
      stroke-dasharray={circumference}
      stroke-dashoffset={strokeDashoffset}
      stroke-linecap="round"
      transform="rotate(-90 50 50)"
    />
    
    <!-- Animated endpoint (like a clock hand) -->
    {#if $progress > 0}
      {@const endAngle = ($progress / 100) * 360 - 90}
      {@const endRad = endAngle * (Math.PI / 180)}
      {@const endX = 50 + 45 * Math.cos(endRad)}
      {@const endY = 50 + 45 * Math.sin(endRad)}
      <circle
        cx={endX}
        cy={endY}
        r="5"
        class="ring-endpoint"
      />
    {/if}
  </svg>
  
  <!-- Center content -->
  <div class="ring-content">
    <span class="ring-icon">{icon}</span>
    <span class="ring-percentage">{Math.round($progress)}%</span>
  </div>
  
  <!-- Label -->
  <div class="ring-label">
    <span class="skill-name">{skill}</span>
    {#if episodes > 0}
      <span class="episodes">
        <span class="episode-icon">🔄</span>
        {episodes.toLocaleString()} episodes
      </span>
    {/if}
  </div>
</div>

<style>
  .skill-ring {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-sm);
    position: relative;
  }
  
  .ring-svg {
    width: var(--size);
    height: var(--size);
    filter: drop-shadow(var(--shadow-md));
  }
  
  .tick-mark {
    stroke: var(--text-muted);
    stroke-width: 1;
    opacity: 0.3;
  }
  
  .tick-mark.main-tick {
    stroke-width: 2;
    opacity: 0.5;
  }
  
  .ring-bg {
    stroke: var(--bg-tertiary);
  }
  
  .ring-progress {
    stroke: var(--color);
    transition: stroke var(--transition-slow);
    filter: drop-shadow(0 0 8px color-mix(in srgb, var(--color) 50%, transparent));
  }
  
  .ring-endpoint {
    fill: var(--color);
    filter: drop-shadow(0 0 6px var(--color));
  }
  
  .ring-content {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    margin-top: -10px; /* Adjust for label space */
  }
  
  .ring-icon {
    font-size: 1.8rem;
    margin-bottom: var(--space-xs);
  }
  
  .ring-percentage {
    font-family: var(--font-mono);
    font-size: 1.4rem;
    font-weight: 700;
    color: var(--text-primary);
  }
  
  .ring-label {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2px;
    margin-top: var(--space-xs);
  }
  
  .skill-name {
    font-family: var(--font-display);
    font-size: 1rem;
    font-weight: 600;
    color: var(--text-primary);
  }
  
  .episodes {
    display: flex;
    align-items: center;
    gap: 4px;
    font-family: var(--font-mono);
    font-size: 0.7rem;
    color: var(--text-secondary);
    background: var(--bg-tertiary);
    padding: 2px 8px;
    border-radius: var(--radius-sm);
  }
  
  .episode-icon {
    font-size: 0.8rem;
  }
</style>
