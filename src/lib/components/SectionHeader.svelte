<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { fly } from 'svelte/transition';
  
  export let title: string;
  export let subtitle: string = '';
  export let icon: string = '⏰';
  export let showMiniClock: boolean = true;
  
  let now = new Date();
  let intervalId: number;
  
  $: hours = now.getHours() % 12;
  $: minutes = now.getMinutes();
  $: seconds = now.getSeconds();
  
  $: hourAngle = (hours * 30) + (minutes * 0.5);
  $: minuteAngle = minutes * 6;
  $: secondAngle = seconds * 6;
  
  onMount(() => {
    if (showMiniClock) {
      intervalId = setInterval(() => {
        now = new Date();
      }, 1000);
    }
  });
  
  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

<header class="section-header" in:fly={{ y: 30, duration: 600 }}>
  {#if showMiniClock}
    <div class="mini-clock-wrapper">
      <svg viewBox="0 0 50 50" class="header-clock">
        <circle cx="25" cy="25" r="22" class="clock-face" />
        <circle cx="25" cy="25" r="22" class="clock-border" fill="none" stroke-width="2" />
        
        {#each Array(12) as _, i}
          {@const angle = (i * 30 - 90) * (Math.PI / 180)}
          {@const x1 = 25 + 18 * Math.cos(angle)}
          {@const y1 = 25 + 18 * Math.sin(angle)}
          {@const x2 = 25 + 15 * Math.cos(angle)}
          {@const y2 = 25 + 15 * Math.sin(angle)}
          <line {x1} {y1} {x2} {y2} class="tick" stroke-width={i % 3 === 0 ? 1.5 : 0.5} />
        {/each}
        
        <line 
          x1="25" y1="25" x2="25" y2="12"
          class="hour-hand"
          stroke-width="2"
          stroke-linecap="round"
          style="transform-origin: 25px 25px; transform: rotate({hourAngle}deg)"
        />
        <line 
          x1="25" y1="25" x2="25" y2="8"
          class="minute-hand"
          stroke-width="1.5"
          stroke-linecap="round"
          style="transform-origin: 25px 25px; transform: rotate({minuteAngle}deg)"
        />
        <line 
          x1="25" y1="28" x2="25" y2="6"
          class="second-hand"
          stroke-width="1"
          stroke-linecap="round"
          style="transform-origin: 25px 25px; transform: rotate({secondAngle}deg)"
        />
        
        <circle cx="25" cy="25" r="3" class="center-dot" />
      </svg>
    </div>
  {:else}
    <span class="title-icon">{icon}</span>
  {/if}
  
  <div class="header-content">
    <h2 class="section-title">{title}</h2>
    {#if subtitle}
      <p class="section-subtitle">{subtitle}</p>
    {/if}
  </div>
  
  <div class="header-decoration">
    <span class="deco-line"></span>
    <span class="deco-dot"></span>
  </div>
</header>

<style>
  .section-header {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-md);
    margin-bottom: var(--space-xl);
    text-align: center;
  }
  
  .mini-clock-wrapper {
    position: relative;
  }
  
  .header-clock {
    width: 60px;
    height: 60px;
    filter: drop-shadow(var(--shadow-sm));
  }
  
  .clock-face {
    fill: var(--bg-secondary);
  }
  
  .clock-border {
    stroke: var(--clock-border);
  }
  
  .tick {
    stroke: var(--clock-tick);
  }
  
  .hour-hand {
    stroke: var(--clock-hour);
  }
  
  .minute-hand {
    stroke: var(--clock-minute);
  }
  
  .second-hand {
    stroke: var(--clock-second);
  }
  
  .center-dot {
    fill: var(--clock-second);
  }
  
  .title-icon {
    font-size: 2.5rem;
  }
  
  .header-content {
    display: flex;
    flex-direction: column;
    gap: var(--space-xs);
  }
  
  .section-title {
    font-size: 2.5rem;
    font-family: var(--font-display);
    background: linear-gradient(135deg, var(--text-primary), var(--accent-primary));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  
  .section-subtitle {
    color: var(--text-secondary);
    font-size: 1.1rem;
  }
  
  .header-decoration {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    margin-top: var(--space-sm);
  }
  
  .deco-line {
    width: 60px;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--accent-primary), transparent);
  }
  
  .deco-dot {
    width: 8px;
    height: 8px;
    background: var(--accent-primary);
    border-radius: 50%;
    animation: pulse 2s ease-in-out infinite;
  }
  
  @media (max-width: 600px) {
    .section-title {
      font-size: 1.8rem;
    }
  }
</style>
