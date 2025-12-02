<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  
  export let size: 'sm' | 'md' | 'lg' | 'xl' = 'md';
  export let showDate: boolean = false;
  export let showMilliseconds: boolean = true;
  export let label: string = '';
  
  let now = Date.now();
  let animationId: number;
  
  function update() {
    now = Date.now();
    animationId = requestAnimationFrame(update);
  }
  
  onMount(() => {
    update();
  });
  
  onDestroy(() => {
    if (animationId) cancelAnimationFrame(animationId);
  });
  
  $: date = new Date(now);
  $: hours = date.getHours().toString().padStart(2, '0');
  $: minutes = date.getMinutes().toString().padStart(2, '0');
  $: seconds = date.getSeconds().toString().padStart(2, '0');
  $: milliseconds = date.getMilliseconds().toString().padStart(3, '0');
  $: dateStr = date.toLocaleDateString('en-CA'); // YYYY-MM-DD format
</script>

<div class="live-time {size}">
  {#if label}
    <span class="label">{label}</span>
  {/if}
  {#if showDate}
    <span class="date">{dateStr}</span>
    <span class="separator">T</span>
  {/if}
  <span class="time">
    <span class="unit">{hours}</span><span class="colon">:</span><span class="unit">{minutes}</span><span class="colon">:</span><span class="unit">{seconds}</span>{#if showMilliseconds}<span class="dot">.</span><span class="ms">{milliseconds}</span>{/if}
  </span>
</div>

<style>
  .live-time {
    font-family: var(--font-mono);
    font-variant-numeric: tabular-nums;
    display: inline-flex;
    align-items: baseline;
    gap: 0.25em;
  }
  
  .label {
    color: var(--text-muted);
    margin-right: 0.5em;
  }
  
  .date {
    color: var(--text-muted);
  }
  
  .separator {
    color: var(--text-muted);
  }
  
  .time {
    color: var(--text);
  }
  
  .colon, .dot {
    color: var(--text-muted);
  }
  
  .ms {
    color: var(--accent);
  }
  
  /* Sizes */
  .sm { font-size: 0.875rem; }
  .md { font-size: 1.25rem; }
  .lg { font-size: 2rem; }
  .xl { font-size: 4rem; }
  
  @media (max-width: 600px) {
    .xl { font-size: 2.5rem; }
    .lg { font-size: 1.5rem; }
  }
</style>
