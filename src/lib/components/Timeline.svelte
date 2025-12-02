<script lang="ts">
  import { fly, scale } from 'svelte/transition';
  import { onMount } from 'svelte';
  
  interface Milestone {
    id: string;
    year: string;
    title: string;
    description: string;
    icon: string;
    color?: string;
  }
  
  export let milestones: Milestone[] = [
    {
      id: '1',
      year: '2020',
      title: 'Started Coding',
      description: 'First steps into programming with Python and web development.',
      icon: '🚀',
      color: 'var(--accent-primary)'
    },
    {
      id: '2',
      year: '2021',
      title: 'Discovered Godot',
      description: 'Began game development journey with Godot Engine.',
      icon: '🎮',
      color: 'var(--skill-godot)'
    },
    {
      id: '3',
      year: '2022',
      title: 'Mobile with Flutter',
      description: 'Expanded into cross-platform mobile development.',
      icon: '📱',
      color: 'var(--skill-flutter)'
    },
    {
      id: '4',
      year: '2023',
      title: 'ML & AI Exploration',
      description: 'Started working with Unity ML-Agents and reinforcement learning.',
      icon: '🤖',
      color: 'var(--skill-ml)'
    },
    {
      id: '5',
      year: '2024',
      title: 'Full Stack Growth',
      description: 'Integrated frontend and backend skills for complete solutions.',
      icon: '⚡',
      color: 'var(--accent-tertiary)'
    }
  ];
  
  let selectedMilestone: Milestone | null = null;
  let visible = false;
  
  function selectMilestone(milestone: Milestone) {
    selectedMilestone = selectedMilestone?.id === milestone.id ? null : milestone;
  }
  
  onMount(() => {
    visible = true;
  });
</script>

<section class="timeline-section" id="timeline">
  <h2 class="section-title" in:fly={{ y: 30, duration: 600 }}>
    <span class="title-icon">⏳</span>
    Timeline of Growth
  </h2>
  <p class="section-subtitle" in:fly={{ y: 30, duration: 600, delay: 100 }}>
    My coding journey through time
  </p>
  
  <div class="timeline-container">
    <!-- Timeline line -->
    <div class="timeline-line"></div>
    
    <!-- Milestones -->
    <div class="milestones">
      {#each milestones as milestone, i}
        <div 
          class="milestone"
          class:active={selectedMilestone?.id === milestone.id}
          style="--delay: {i * 100}ms; --color: {milestone.color || 'var(--accent-primary)'}"
          in:fly={{ y: 50, duration: 500, delay: 200 + i * 100 }}
        >
          <button 
            class="milestone-marker"
            on:click={() => selectMilestone(milestone)}
            aria-expanded={selectedMilestone?.id === milestone.id}
            aria-label="View details for {milestone.title}"
          >
            <span class="marker-icon">{milestone.icon}</span>
            <span class="marker-year">{milestone.year}</span>
          </button>
          
          <div class="milestone-content">
            <h3 class="milestone-title">{milestone.title}</h3>
            
            {#if selectedMilestone?.id === milestone.id}
              <div 
                class="milestone-details"
                in:scale={{ duration: 300 }}
              >
                <p>{milestone.description}</p>
              </div>
            {/if}
          </div>
        </div>
      {/each}
    </div>
  </div>
  
  <!-- Expanded detail card -->
  {#if selectedMilestone}
    <div 
      class="detail-card"
      in:fly={{ y: 20, duration: 400 }}
      style="--color: {selectedMilestone.color}"
    >
      <div class="detail-header">
        <span class="detail-icon">{selectedMilestone.icon}</span>
        <div>
          <span class="detail-year">{selectedMilestone.year}</span>
          <h3>{selectedMilestone.title}</h3>
        </div>
      </div>
      <p>{selectedMilestone.description}</p>
      <button class="close-btn" on:click={() => selectedMilestone = null}>
        ✕
      </button>
    </div>
  {/if}
</section>

<style>
  .timeline-section {
    padding: var(--space-2xl) 0;
    position: relative;
  }
  
  .section-title {
    text-align: center;
    font-size: 2.5rem;
    margin-bottom: var(--space-sm);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: var(--space-md);
  }
  
  .title-icon {
    font-size: 2rem;
  }
  
  .section-subtitle {
    text-align: center;
    color: var(--text-secondary);
    font-size: 1.1rem;
    margin-bottom: var(--space-xl);
  }
  
  .timeline-container {
    position: relative;
    max-width: 900px;
    margin: 0 auto;
    padding: var(--space-lg) 0;
  }
  
  .timeline-line {
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    height: 4px;
    background: linear-gradient(
      90deg,
      var(--accent-primary) 0%,
      var(--accent-tertiary) 50%,
      var(--accent-secondary) 100%
    );
    border-radius: 2px;
    transform: translateY(-50%);
  }
  
  .milestones {
    display: flex;
    justify-content: space-between;
    position: relative;
    z-index: 1;
  }
  
  .milestone {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    animation: fadeIn 0.5s ease var(--delay) both;
  }
  
  .milestone-marker {
    width: 70px;
    height: 70px;
    border-radius: var(--radius-full);
    background: var(--bg-primary);
    border: 4px solid var(--color);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all var(--transition-normal);
    box-shadow: var(--shadow-md);
  }
  
  .milestone-marker:hover,
  .milestone.active .milestone-marker {
    transform: scale(1.15);
    background: var(--color);
    box-shadow: 0 0 20px color-mix(in srgb, var(--color) 50%, transparent);
  }
  
  .milestone-marker:hover .marker-icon,
  .milestone.active .marker-icon {
    transform: scale(1.2);
  }
  
  .milestone-marker:hover .marker-year,
  .milestone.active .marker-year {
    color: white;
  }
  
  .marker-icon {
    font-size: 1.5rem;
    transition: transform var(--transition-fast);
  }
  
  .marker-year {
    font-family: var(--font-mono);
    font-size: 0.7rem;
    font-weight: 600;
    color: var(--text-secondary);
    transition: color var(--transition-fast);
  }
  
  .milestone-content {
    margin-top: var(--space-md);
    text-align: center;
    max-width: 150px;
  }
  
  .milestone-title {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-primary);
  }
  
  .milestone-details {
    margin-top: var(--space-sm);
    font-size: 0.8rem;
    color: var(--text-secondary);
  }
  
  /* Detail card */
  .detail-card {
    max-width: 500px;
    margin: var(--space-xl) auto 0;
    background: var(--bg-secondary);
    border-radius: var(--radius-lg);
    padding: var(--space-lg);
    box-shadow: var(--shadow-lg);
    border-left: 4px solid var(--color);
    position: relative;
  }
  
  .detail-header {
    display: flex;
    align-items: center;
    gap: var(--space-md);
    margin-bottom: var(--space-md);
  }
  
  .detail-icon {
    font-size: 2.5rem;
  }
  
  .detail-year {
    font-family: var(--font-mono);
    font-size: 0.8rem;
    color: var(--color);
    font-weight: 600;
  }
  
  .detail-header h3 {
    font-size: 1.3rem;
    margin-top: var(--space-xs);
  }
  
  .detail-card p {
    color: var(--text-secondary);
    line-height: 1.7;
  }
  
  .close-btn {
    position: absolute;
    top: var(--space-md);
    right: var(--space-md);
    background: none;
    border: none;
    font-size: 1.2rem;
    color: var(--text-muted);
    cursor: pointer;
    transition: color var(--transition-fast);
  }
  
  .close-btn:hover {
    color: var(--text-primary);
  }
  
  /* Responsive */
  @media (max-width: 768px) {
    .timeline-line {
      left: 35px;
      right: auto;
      top: 0;
      bottom: 0;
      width: 4px;
      height: auto;
      transform: none;
    }
    
    .milestones {
      flex-direction: column;
      gap: var(--space-xl);
      padding-left: 80px;
    }
    
    .milestone {
      flex-direction: row;
      align-items: flex-start;
    }
    
    .milestone-marker {
      position: absolute;
      left: 0;
    }
    
    .milestone-content {
      margin-top: 0;
      text-align: left;
      max-width: none;
    }
  }
</style>
