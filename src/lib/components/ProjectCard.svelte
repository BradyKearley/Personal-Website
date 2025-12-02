<script lang="ts">
  import { fly, scale } from 'svelte/transition';
  
  export let title: string;
  export let description: string;
  export let tech: string[] = [];
  export let image: string = '';
  export let link: string = '';
  export let github: string = '';
  export let startDate: string = '';
  export let endDate: string = '';
  export let status: 'completed' | 'in-progress' | 'planned' = 'completed';
  
  let isHovered = false;
  let isExpanded = false;
  
  function toggleExpand() {
    isExpanded = !isExpanded;
  }
  
  // Calculate "clock time" from dates for visual effect
  function getClockPosition(date: string): { hour: number; minute: number } {
    const d = new Date(date);
    return {
      hour: (d.getMonth() % 12) || 12,
      minute: Math.floor((d.getDate() / 31) * 60)
    };
  }
  
  $: clockPos = startDate ? getClockPosition(startDate) : { hour: 12, minute: 0 };
  $: hourAngle = (clockPos.hour * 30) + (clockPos.minute * 0.5);
  $: minuteAngle = clockPos.minute * 6;
</script>

<article 
  class="project-card"
  class:expanded={isExpanded}
  class:hovered={isHovered}
  on:mouseenter={() => isHovered = true}
  on:mouseleave={() => isHovered = false}
  in:fly={{ y: 40, duration: 500 }}
>
  <!-- Clock-style header -->
  <div class="card-clock">
    <svg viewBox="0 0 60 60" class="mini-clock">
      <!-- Clock face -->
      <circle cx="30" cy="30" r="28" class="clock-face" />
      <circle cx="30" cy="30" r="28" class="clock-border" fill="none" stroke-width="2" />
      
      <!-- Tick marks -->
      {#each Array(12) as _, i}
        {@const angle = (i * 30 - 90) * (Math.PI / 180)}
        {@const x1 = 30 + 24 * Math.cos(angle)}
        {@const y1 = 30 + 24 * Math.sin(angle)}
        {@const x2 = 30 + 20 * Math.cos(angle)}
        {@const y2 = 30 + 20 * Math.sin(angle)}
        <line {x1} {y1} {x2} {y2} class="tick" stroke-width={i % 3 === 0 ? 2 : 1} />
      {/each}
      
      <!-- Hour hand -->
      <line 
        x1="30" y1="30" x2="30" y2="16"
        class="hour-hand"
        style="transform-origin: 30px 30px; transform: rotate({hourAngle}deg)"
        stroke-width="3"
        stroke-linecap="round"
      />
      
      <!-- Minute hand -->
      <line 
        x1="30" y1="30" x2="30" y2="10"
        class="minute-hand"
        style="transform-origin: 30px 30px; transform: rotate({minuteAngle}deg)"
        stroke-width="2"
        stroke-linecap="round"
      />
      
      <!-- Center -->
      <circle cx="30" cy="30" r="3" class="center-dot" />
    </svg>
    
    <!-- Status indicator -->
    <span class="status-badge" class:completed={status === 'completed'} class:in-progress={status === 'in-progress'}>
      {#if status === 'completed'}✓{:else if status === 'in-progress'}⏳{:else}📅{/if}
    </span>
  </div>
  
  <!-- Card content -->
  <div class="card-body">
    <header class="card-header">
      <h3 class="card-title">{title}</h3>
      {#if startDate}
        <div class="card-dates">
          <span class="date-label">📅</span>
          <span>{startDate}{endDate ? ` → ${endDate}` : ' → Present'}</span>
        </div>
      {/if}
    </header>
    
    <p class="card-description">{description}</p>
    
    <!-- Tech stack -->
    {#if tech.length > 0}
      <div class="tech-stack">
        {#each tech as t}
          <span class="tech-tag">{t}</span>
        {/each}
      </div>
    {/if}
    
    <!-- Expanded content -->
    {#if isExpanded}
      <div class="expanded-content" in:scale={{ duration: 300 }}>
        {#if image}
          <img src={image} alt="{title} screenshot" class="project-image" />
        {/if}
        
        <div class="card-actions">
          {#if link}
            <a href={link} target="_blank" rel="noopener noreferrer" class="action-btn primary">
              🔗 View Project
            </a>
          {/if}
          {#if github}
            <a href={github} target="_blank" rel="noopener noreferrer" class="action-btn secondary">
              💻 Source Code
            </a>
          {/if}
        </div>
      </div>
    {/if}
    
    <button class="expand-btn" on:click={toggleExpand}>
      {isExpanded ? '▲ Less' : '▼ More'}
    </button>
  </div>
</article>

<style>
  .project-card {
    background: var(--bg-secondary);
    border-radius: var(--radius-lg);
    overflow: hidden;
    box-shadow: var(--shadow-md);
    transition: all var(--transition-normal);
    display: flex;
    flex-direction: column;
  }
  
  .project-card:hover,
  .project-card.hovered {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
  }
  
  .project-card.expanded {
    grid-column: span 2;
  }
  
  @media (max-width: 768px) {
    .project-card.expanded {
      grid-column: span 1;
    }
  }
  
  /* Clock header */
  .card-clock {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: var(--space-md);
    background: var(--bg-tertiary);
    border-bottom: 1px solid var(--bg-tertiary);
  }
  
  .mini-clock {
    width: 50px;
    height: 50px;
  }
  
  .clock-face {
    fill: var(--clock-face);
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
  
  .center-dot {
    fill: var(--clock-second);
  }
  
  .status-badge {
    width: 32px;
    height: 32px;
    border-radius: var(--radius-full);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.9rem;
    background: var(--bg-secondary);
    border: 2px solid var(--text-muted);
  }
  
  .status-badge.completed {
    background: var(--accent-success);
    border-color: var(--accent-success);
    color: white;
  }
  
  .status-badge.in-progress {
    background: var(--accent-warm);
    border-color: var(--accent-warm);
  }
  
  /* Card body */
  .card-body {
    padding: var(--space-lg);
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  
  .card-header {
    margin-bottom: var(--space-md);
  }
  
  .card-title {
    font-size: 1.3rem;
    font-family: var(--font-display);
    margin-bottom: var(--space-xs);
  }
  
  .card-dates {
    display: flex;
    align-items: center;
    gap: var(--space-xs);
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--text-secondary);
  }
  
  .card-description {
    color: var(--text-secondary);
    font-size: 0.95rem;
    line-height: 1.6;
    margin-bottom: var(--space-md);
  }
  
  /* Tech stack */
  .tech-stack {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-xs);
    margin-bottom: var(--space-md);
  }
  
  .tech-tag {
    font-family: var(--font-mono);
    font-size: 0.75rem;
    padding: 4px 10px;
    background: var(--bg-tertiary);
    color: var(--accent-primary);
    border-radius: var(--radius-sm);
    transition: all var(--transition-fast);
  }
  
  .tech-tag:hover {
    background: var(--accent-primary);
    color: white;
  }
  
  /* Expanded content */
  .expanded-content {
    margin-top: var(--space-md);
    padding-top: var(--space-md);
    border-top: 1px solid var(--bg-tertiary);
  }
  
  .project-image {
    width: 100%;
    border-radius: var(--radius-md);
    margin-bottom: var(--space-md);
  }
  
  .card-actions {
    display: flex;
    gap: var(--space-sm);
    flex-wrap: wrap;
  }
  
  .action-btn {
    display: inline-flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-sm) var(--space-md);
    border-radius: var(--radius-md);
    font-size: 0.9rem;
    font-weight: 500;
    transition: all var(--transition-fast);
    text-decoration: none;
  }
  
  .action-btn.primary {
    background: var(--accent-primary);
    color: white;
  }
  
  .action-btn.primary:hover {
    background: var(--accent-tertiary);
  }
  
  .action-btn.secondary {
    background: var(--bg-tertiary);
    color: var(--text-primary);
  }
  
  .action-btn.secondary:hover {
    background: var(--text-muted);
  }
  
  /* Expand button */
  .expand-btn {
    margin-top: auto;
    padding: var(--space-sm);
    background: none;
    border: none;
    color: var(--text-secondary);
    font-size: 0.85rem;
    cursor: pointer;
    transition: color var(--transition-fast);
  }
  
  .expand-btn:hover {
    color: var(--accent-primary);
  }
</style>
