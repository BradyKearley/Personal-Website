<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  
  interface Section {
    id: string;
    label: string;
    hour: number;
  }
  
  export let sections: Section[] = [
    { id: 'about', label: 'About', hour: 12 },
    { id: 'skills', label: 'Skills', hour: 2 },
    { id: 'projects', label: 'Projects', hour: 4 },
    { id: 'education', label: 'Education', hour: 8 },
    { id: 'contact', label: 'Contact', hour: 10 }
  ];
  
  export let size: number = 400;
  export let interactive: boolean = true;
  
  let now = new Date();
  let intervalId: number;
  let hoveredSection: string | null = null;
  let isSpinning = false;
  
  // Calculate hand angles
  $: hours = now.getHours() % 12;
  $: minutes = now.getMinutes();
  $: seconds = now.getSeconds();
  
  $: hourAngle = (hours * 30) + (minutes * 0.5); // 30° per hour + adjustment for minutes
  $: minuteAngle = minutes * 6; // 6° per minute
  $: secondAngle = seconds * 6; // 6° per second
  
  // Clock dimensions
  $: radius = size / 2;
  $: center = radius;
  $: clockRadius = radius - 20;
  
  function getTickPosition(hour: number, radiusOffset: number = 0) {
    const angle = ((hour * 30) - 90) * (Math.PI / 180);
    const r = clockRadius - radiusOffset;
    return {
      x: center + r * Math.cos(angle),
      y: center + r * Math.sin(angle)
    };
  }
  
  function handleSectionClick(sectionId: string) {
    if (!interactive) return;
    isSpinning = true;
    setTimeout(() => {
      isSpinning = false;
      const element = document.getElementById(sectionId);
      element?.scrollIntoView({ behavior: 'smooth' });
    }, 600);
  }
  
  onMount(() => {
    intervalId = setInterval(() => {
      now = new Date();
    }, 1000);
  });
  
  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

<div 
  class="clock-container"
  class:spinning={isSpinning}
  style="--size: {size}px"
  in:scale={{ duration: 800, delay: 200 }}
>
  <svg 
    viewBox="0 0 {size} {size}" 
    class="clock"
    aria-label="Interactive navigation clock"
  >
    <!-- Outer glow -->
    <defs>
      <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="4" result="coloredBlur"/>
        <feMerge>
          <feMergeNode in="coloredBlur"/>
          <feMergeNode in="SourceGraphic"/>
        </feMerge>
      </filter>
      <linearGradient id="clockGradient" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" style="stop-color: var(--clock-face)" />
        <stop offset="100%" style="stop-color: var(--bg-secondary)" />
      </linearGradient>
    </defs>
    
    <!-- Clock face background -->
    <circle 
      cx={center} 
      cy={center} 
      r={clockRadius}
      class="clock-face"
      fill="url(#clockGradient)"
    />
    
    <!-- Clock border -->
    <circle 
      cx={center} 
      cy={center} 
      r={clockRadius}
      class="clock-border"
      fill="none"
      stroke-width="4"
    />
    
    <!-- Hour ticks -->
    {#each Array(12) as _, i}
      {@const isMainTick = i % 3 === 0}
      {@const outerPos = getTickPosition(i, 0)}
      {@const innerPos = getTickPosition(i, isMainTick ? 25 : 15)}
      <line 
        x1={outerPos.x} 
        y1={outerPos.y}
        x2={innerPos.x}
        y2={innerPos.y}
        class="tick"
        class:main-tick={isMainTick}
        stroke-width={isMainTick ? 3 : 2}
      />
    {/each}
    
    <!-- Section labels (interactive) - using foreignObject for better hover -->
    {#each sections as section}
      {@const pos = getTickPosition(section.hour, 55)}
      <foreignObject 
        x={pos.x - 32} 
        y={pos.y - 32} 
        width="64" 
        height="64"
      >
        <button
          class="section-button"
          class:hovered={hoveredSection === section.id}
          on:click={() => handleSectionClick(section.id)}
          on:mouseenter={() => hoveredSection = section.id}
          on:mouseleave={() => hoveredSection = null}
          aria-label="Navigate to {section.label}"
        >
          <span class="section-label-text">{section.label}</span>
        </button>
      </foreignObject>
    {/each}
    
    <!-- Hour hand -->
    <line 
      x1={center} 
      y1={center}
      x2={center}
      y2={center - clockRadius * 0.5}
      class="hand hour-hand"
      style="transform-origin: {center}px {center}px; transform: rotate({hourAngle}deg)"
      stroke-linecap="round"
    />
    
    <!-- Minute hand -->
    <line 
      x1={center} 
      y1={center}
      x2={center}
      y2={center - clockRadius * 0.7}
      class="hand minute-hand"
      style="transform-origin: {center}px {center}px; transform: rotate({minuteAngle}deg)"
      stroke-linecap="round"
    />
    
    <!-- Second hand -->
    <line 
      x1={center} 
      y1={center + 20}
      x2={center}
      y2={center - clockRadius * 0.75}
      class="hand second-hand"
      style="transform-origin: {center}px {center}px; transform: rotate({secondAngle}deg)"
      stroke-linecap="round"
      filter="url(#glow)"
    />
    
    <!-- Center dot -->
    <circle 
      cx={center} 
      cy={center} 
      r="12"
      class="center-dot"
    />
    <circle 
      cx={center} 
      cy={center} 
      r="6"
      class="center-dot-inner"
    />
  </svg>
  
  <!-- Digital time display -->
  <div class="digital-time" in:fly={{ y: 20, duration: 500, delay: 600 }}>
    <time datetime={now.toISOString()}>
      {now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' })}
    </time>
  </div>
</div>

<style>
  .clock-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-lg);
  }
  
  .clock {
    width: var(--size);
    height: var(--size);
    filter: drop-shadow(var(--shadow-clock));
    transition: transform var(--transition-clock);
  }
  
  .clock-container.spinning .clock {
    animation: clockSpin 0.6s ease-in-out;
  }
  
  .clock-face {
    transition: fill var(--transition-slow);
  }
  
  .clock-border {
    stroke: var(--clock-border);
    transition: stroke var(--transition-slow);
  }
  
  .tick {
    stroke: var(--clock-tick);
    transition: stroke var(--transition-slow);
  }
  
  .main-tick {
    stroke: var(--clock-border);
  }
  
  /* Section buttons - using HTML for better hover */
  .section-button {
    width: 56px;
    height: 56px;
    border-radius: 50%;
    border: 2px solid var(--clock-tick);
    background: var(--bg-secondary);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    margin: 4px;
    box-shadow: var(--shadow-sm);
  }
  
  .section-button:hover,
  .section-button.hovered {
    background: var(--accent-primary);
    border-color: var(--accent-primary);
    transform: scale(1.15);
    box-shadow: 0 0 20px color-mix(in srgb, var(--accent-primary) 50%, transparent);
  }
  
  .section-label-text {
    font-family: var(--font-display);
    font-size: 10px;
    font-weight: 600;
    color: var(--text-primary);
    text-transform: uppercase;
    letter-spacing: 0.5px;
    transition: color 0.2s ease;
  }
  
  .section-button:hover .section-label-text,
  .section-button.hovered .section-label-text {
    color: white;
  }
  
  /* Clock hands */
  .hand {
    transition: stroke var(--transition-slow);
  }
  
  .hour-hand {
    stroke: var(--clock-hour);
    stroke-width: 6;
  }
  
  .minute-hand {
    stroke: var(--clock-minute);
    stroke-width: 4;
  }
  
  .second-hand {
    stroke: var(--clock-second);
    stroke-width: 2;
    transition: transform 0.1s cubic-bezier(0.4, 2.08, 0.55, 0.44);
  }
  
  .center-dot {
    fill: var(--clock-border);
    transition: fill var(--transition-slow);
  }
  
  .center-dot-inner {
    fill: var(--clock-second);
    transition: fill var(--transition-slow);
  }
  
  /* Digital time */
  .digital-time {
    font-family: var(--font-mono);
    font-size: 1.5rem;
    color: var(--text-secondary);
    background: var(--bg-secondary);
    padding: var(--space-sm) var(--space-lg);
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-sm);
    transition: all var(--transition-slow);
  }
  
  .digital-time time {
    letter-spacing: 2px;
  }
  
  /* Responsive */
  @media (max-width: 600px) {
    .clock-container {
      --size: 300px !important;
    }
    
    .section-text {
      font-size: 9px;
    }
    
    .digital-time {
      font-size: 1.2rem;
    }
  }
</style>
