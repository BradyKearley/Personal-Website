<script lang="ts">
  import { fly, scale } from 'svelte/transition';
  import { onMount } from 'svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  
  // Calculate time until graduation (April 2028)
  const graduationDate = new Date('2028-04-30');
  const now = new Date();
  const totalMonths = (graduationDate.getFullYear() - now.getFullYear()) * 12 + (graduationDate.getMonth() - now.getMonth());
  const totalDays = Math.floor((graduationDate.getTime() - now.getTime()) / (1000 * 60 * 60 * 24));
  
  // GPA as clock progress (11.33/12 = ~94.4%)
  const gpaProgress = tweened(0, { duration: 1500, easing: cubicOut });
  const gpaPercent = (11.33 / 12) * 100;
  
  const coursework = [
    { name: 'Java Programming', icon: '☕', completed: true },
    { name: 'Data Structures & Algorithms', icon: '🌳', completed: true },
    { name: 'Object-Oriented Programming', icon: '🧩', completed: true },
    { name: 'Discrete Mathematics', icon: '🔢', completed: true },
  ];
  
  let visible = false;
  
  onMount(() => {
    visible = true;
    setTimeout(() => gpaProgress.set(gpaPercent), 300);
  });
  
  // Clock angle for GPA visualization
  $: gpaAngle = ($gpaProgress / 100) * 360;
</script>

<section class="education-section" id="education">
  <h2 class="section-title" in:fly={{ y: 30, duration: 600 }}>
    <span class="title-icon">🎓</span>
    Education
  </h2>
  <p class="section-subtitle" in:fly={{ y: 30, duration: 600, delay: 100 }}>
    Time invested in learning
  </p>
  
  <div class="education-grid">
    <!-- Main Education Card with Clock -->
    <div class="edu-card main-card" in:fly={{ y: 40, duration: 600, delay: 200 }}>
      <div class="edu-clock">
        <svg viewBox="0 0 120 120" class="gpa-clock">
          <!-- Clock face -->
          <circle cx="60" cy="60" r="54" class="clock-bg" />
          <circle cx="60" cy="60" r="54" class="clock-border" fill="none" stroke-width="3" />
          
          <!-- Hour marks -->
          {#each Array(12) as _, i}
            {@const angle = (i * 30 - 90) * (Math.PI / 180)}
            {@const x1 = 60 + 48 * Math.cos(angle)}
            {@const y1 = 60 + 48 * Math.sin(angle)}
            {@const x2 = 60 + 42 * Math.cos(angle)}
            {@const y2 = 60 + 42 * Math.sin(angle)}
            <line {x1} {y1} {x2} {y2} class="tick" stroke-width={i % 3 === 0 ? 2 : 1} />
          {/each}
          
          <!-- GPA progress arc -->
          <circle 
            cx="60" 
            cy="60" 
            r="48" 
            class="gpa-arc"
            fill="none"
            stroke-width="6"
            stroke-dasharray={2 * Math.PI * 48}
            stroke-dashoffset={2 * Math.PI * 48 * (1 - $gpaProgress / 100)}
            stroke-linecap="round"
            transform="rotate(-90 60 60)"
          />
          
          <!-- GPA hand -->
          <line 
            x1="60" y1="60" x2="60" y2="18"
            class="gpa-hand"
            stroke-width="3"
            stroke-linecap="round"
            style="transform-origin: 60px 60px; transform: rotate({gpaAngle}deg)"
          />
          
          <!-- Center -->
          <circle cx="60" cy="60" r="8" class="center-dot" />
          <circle cx="60" cy="60" r="4" class="center-inner" />
        </svg>
        
        <div class="gpa-display">
          <span class="gpa-value">11.33</span>
          <span class="gpa-max">/12</span>
        </div>
      </div>
      
      <div class="edu-details">
        <div class="degree-badge">
          <span class="badge-icon">🏅</span>
          <span>Honors Program</span>
        </div>
        
        <h3 class="degree-title">B.Sc. in Computer Science</h3>
        <p class="degree-subtitle">Honors with Co-Op</p>
        
        <div class="university">
          <span class="uni-icon">🏛️</span>
          <span class="uni-name">Carleton University</span>
        </div>
        
        <div class="graduation-countdown">
          <div class="countdown-item">
            <span class="countdown-value">{totalMonths}</span>
            <span class="countdown-label">months</span>
          </div>
          <div class="countdown-divider">:</div>
          <div class="countdown-item">
            <span class="countdown-value">{totalDays % 30}</span>
            <span class="countdown-label">days</span>
          </div>
          <span class="countdown-text">until graduation (April 2028)</span>
        </div>
      </div>
    </div>
    
    <!-- Co-Op Availability Card -->
    <div class="edu-card coop-card" in:fly={{ y: 40, duration: 600, delay: 300 }}>
      <div class="card-header">
        <span class="card-icon">💼</span>
        <h3>Co-Op Stream</h3>
      </div>
      
      <div class="availability">
        <div class="status-indicator available">
          <span class="pulse"></span>
          <span>Available</span>
        </div>
        
        <div class="term-options">
          <div class="term-badge">
            <span class="term-icon">📅</span>
            <span>4 month term</span>
          </div>
          <div class="term-badge">
            <span class="term-icon">📅</span>
            <span>8 month term</span>
          </div>
        </div>
        
        <div class="start-date">
          <span class="clock-icon">⏰</span>
          <span>Starting <strong>Summer 2025</strong></span>
        </div>
      </div>
    </div>
    
    <!-- Coursework Card -->
    <div class="edu-card coursework-card" in:fly={{ y: 40, duration: 600, delay: 400 }}>
      <div class="card-header">
        <span class="card-icon">📚</span>
        <h3>Relevant Coursework</h3>
      </div>
      
      <ul class="course-list">
        {#each coursework as course, i}
          <li 
            class="course-item"
            class:completed={course.completed}
            in:fly={{ x: -20, duration: 400, delay: 500 + i * 100 }}
          >
            <span class="course-icon">{course.icon}</span>
            <span class="course-name">{course.name}</span>
            {#if course.completed}
              <span class="check">✓</span>
            {/if}
          </li>
        {/each}
      </ul>
    </div>
  </div>
</section>

<style>
  .education-section {
    padding: var(--space-2xl) 0;
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
  
  .education-grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    grid-template-rows: auto auto;
    gap: var(--space-lg);
    max-width: 900px;
    margin: 0 auto;
  }
  
  .edu-card {
    background: var(--bg-secondary);
    border-radius: var(--radius-lg);
    padding: var(--space-lg);
    box-shadow: var(--shadow-md);
    transition: transform var(--transition-normal), box-shadow var(--transition-normal);
  }
  
  .edu-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
  }
  
  /* Main Card */
  .main-card {
    grid-row: span 2;
    display: flex;
    gap: var(--space-xl);
    align-items: center;
  }
  
  .edu-clock {
    position: relative;
    flex-shrink: 0;
  }
  
  .gpa-clock {
    width: 140px;
    height: 140px;
  }
  
  .clock-bg {
    fill: var(--clock-face);
  }
  
  .clock-border {
    stroke: var(--clock-border);
  }
  
  .tick {
    stroke: var(--clock-tick);
  }
  
  .gpa-arc {
    stroke: var(--accent-success);
    filter: drop-shadow(0 0 8px var(--accent-success));
    transition: stroke-dashoffset 1.5s ease;
  }
  
  .gpa-hand {
    stroke: var(--accent-success);
  }
  
  .center-dot {
    fill: var(--clock-border);
  }
  
  .center-inner {
    fill: var(--accent-success);
  }
  
  .gpa-display {
    position: absolute;
    bottom: -10px;
    left: 50%;
    transform: translateX(-50%);
    background: var(--bg-tertiary);
    padding: 4px 12px;
    border-radius: var(--radius-md);
    display: flex;
    align-items: baseline;
    gap: 2px;
  }
  
  .gpa-value {
    font-family: var(--font-mono);
    font-size: 1.2rem;
    font-weight: 700;
    color: var(--accent-success);
  }
  
  .gpa-max {
    font-family: var(--font-mono);
    font-size: 0.8rem;
    color: var(--text-secondary);
  }
  
  .edu-details {
    flex: 1;
  }
  
  .degree-badge {
    display: inline-flex;
    align-items: center;
    gap: var(--space-xs);
    background: linear-gradient(135deg, var(--accent-tertiary), var(--accent-primary));
    color: white;
    padding: 4px 12px;
    border-radius: var(--radius-sm);
    font-size: 0.8rem;
    font-weight: 600;
    margin-bottom: var(--space-sm);
  }
  
  .degree-title {
    font-size: 1.5rem;
    margin-bottom: var(--space-xs);
  }
  
  .degree-subtitle {
    color: var(--text-secondary);
    margin-bottom: var(--space-md);
  }
  
  .university {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    margin-bottom: var(--space-md);
  }
  
  .uni-icon {
    font-size: 1.3rem;
  }
  
  .uni-name {
    font-weight: 600;
    color: var(--accent-primary);
  }
  
  .graduation-countdown {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    flex-wrap: wrap;
    background: var(--bg-tertiary);
    padding: var(--space-md);
    border-radius: var(--radius-md);
  }
  
  .countdown-item {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .countdown-value {
    font-family: var(--font-mono);
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--accent-primary);
  }
  
  .countdown-label {
    font-size: 0.7rem;
    color: var(--text-secondary);
    text-transform: uppercase;
  }
  
  .countdown-divider {
    font-family: var(--font-mono);
    font-size: 1.5rem;
    color: var(--text-muted);
    animation: pulse 1s ease-in-out infinite;
  }
  
  .countdown-text {
    font-size: 0.85rem;
    color: var(--text-secondary);
    margin-left: var(--space-sm);
  }
  
  /* Co-Op Card */
  .card-header {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    margin-bottom: var(--space-md);
  }
  
  .card-icon {
    font-size: 1.5rem;
  }
  
  .card-header h3 {
    font-size: 1.1rem;
  }
  
  .availability {
    display: flex;
    flex-direction: column;
    gap: var(--space-md);
  }
  
  .status-indicator {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    font-weight: 600;
  }
  
  .status-indicator.available {
    color: var(--accent-success);
  }
  
  .pulse {
    width: 12px;
    height: 12px;
    background: var(--accent-success);
    border-radius: 50%;
    animation: pulse 1.5s ease-in-out infinite;
    box-shadow: 0 0 10px var(--accent-success);
  }
  
  .term-options {
    display: flex;
    flex-direction: column;
    gap: var(--space-xs);
  }
  
  .term-badge {
    display: flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-xs) var(--space-sm);
    background: var(--bg-tertiary);
    border-radius: var(--radius-sm);
    font-size: 0.85rem;
  }
  
  .start-date {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    padding: var(--space-sm);
    background: var(--bg-tertiary);
    border-radius: var(--radius-md);
    border-left: 3px solid var(--accent-primary);
  }
  
  .clock-icon {
    font-size: 1.2rem;
  }
  
  /* Coursework Card */
  .course-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: var(--space-sm);
  }
  
  .course-item {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    padding: var(--space-sm);
    background: var(--bg-tertiary);
    border-radius: var(--radius-md);
    transition: all var(--transition-fast);
  }
  
  .course-item:hover {
    transform: translateX(4px);
    background: var(--bg-primary);
  }
  
  .course-icon {
    font-size: 1.1rem;
  }
  
  .course-name {
    flex: 1;
    font-size: 0.9rem;
  }
  
  .check {
    color: var(--accent-success);
    font-weight: 700;
  }
  
  /* Responsive */
  @media (max-width: 768px) {
    .education-grid {
      grid-template-columns: 1fr;
    }
    
    .main-card {
      flex-direction: column;
      text-align: center;
    }
    
    .graduation-countdown {
      justify-content: center;
    }
  }
</style>
