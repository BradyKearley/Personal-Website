<script lang="ts">
  import { onMount, onDestroy } from "svelte";

  export let targetDate: Date;
  export let label: string = "";
  export let size: "sm" | "md" | "lg" | "xl" = "md";

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

  $: diff = targetDate.getTime() - now;
  $: isPast = diff < 0;
  $: absDiff = Math.abs(diff);

  $: days = Math.floor(absDiff / (1000 * 60 * 60 * 24));
  $: hours = Math.floor((absDiff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  $: minutes = Math.floor((absDiff % (1000 * 60 * 60)) / (1000 * 60));
  $: seconds = Math.floor((absDiff % (1000 * 60)) / 1000);
  $: milliseconds = absDiff % 1000;
</script>

<div class="countdown {size}">
  {#if label}
    <span class="label">{label}</span>
  {/if}
  <span class="prefix">{isPast ? "+" : "-"}</span>
  <span class="value">{days.toString().padStart(3, "0")}</span><span
    class="unit">d</span
  >
  <span class="value">{hours.toString().padStart(2, "0")}</span><span
    class="unit">h</span
  >
  <span class="value">{minutes.toString().padStart(2, "0")}</span><span
    class="unit">m</span
  >
  <span class="value">{seconds.toString().padStart(2, "0")}</span><span
    class="unit">s</span
  >
  <span class="ms">{milliseconds.toString().padStart(3, "0")}</span><span
    class="unit">ms</span
  >
</div>

<style>
  .countdown {
    font-family: var(--font-mono);
    font-variant-numeric: tabular-nums;
    display: inline-flex;
    align-items: baseline;
    gap: 0.1em;
    flex-wrap: wrap;
    justify-content: center;
  }

  .sm {
    font-size: 0.875rem;
    flex-wrap: nowrap;
    white-space: nowrap;
  }
  .md {
    font-size: 1.25rem;
  }
  .lg {
    font-size: 2rem;
  }
  .xl {
    font-size: 2.5rem;
  }

  .label {
    color: var(--text-muted);
    margin-right: 0.75em;
  }

  .prefix {
    color: var(--accent);
    margin-right: 0.25em;
  }

  .value {
    color: var(--text);
  }

  .unit {
    color: var(--text-muted);
    font-size: 0.6em;
    margin-right: 0.5em;
  }

  .ms {
    color: var(--accent);
  }

  @media (max-width: 600px) {
    .xl {
      font-size: 1.5rem;
    }
    .lg {
      font-size: 1.25rem;
    }
    .sm {
      font-size: 0.75rem;
    }
  }
</style>
