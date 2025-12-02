<script lang="ts">
  import { onMount } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  
  let theme: 'light' | 'dark' = 'light';
  let isAnimating = false;
  
  function toggleTheme() {
    if (isAnimating) return;
    isAnimating = true;
    
    theme = theme === 'light' ? 'dark' : 'light';
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem('theme', theme);
    
    setTimeout(() => {
      isAnimating = false;
    }, 500);
  }
  
  onMount(() => {
    // Check for saved preference or system preference
    const saved = localStorage.getItem('theme') as 'light' | 'dark' | null;
    const systemPrefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    
    theme = saved || (systemPrefersDark ? 'dark' : 'light');
    document.documentElement.setAttribute('data-theme', theme);
  });
</script>

<button 
  class="theme-toggle"
  class:dark={theme === 'dark'}
  class:animating={isAnimating}
  on:click={toggleTheme}
  aria-label="Toggle {theme === 'light' ? 'dark' : 'light'} mode"
  title="Switch to {theme === 'light' ? 'night' : 'day'} mode"
>
  <div class="toggle-track">
    <!-- Sun -->
    <div class="sun" class:active={theme === 'light'}>
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="12" cy="12" r="5"/>
        <line x1="12" y1="1" x2="12" y2="3"/>
        <line x1="12" y1="21" x2="12" y2="23"/>
        <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/>
        <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
        <line x1="1" y1="12" x2="3" y2="12"/>
        <line x1="21" y1="12" x2="23" y2="12"/>
        <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/>
        <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
      </svg>
    </div>
    
    <!-- Moon -->
    <div class="moon" class:active={theme === 'dark'}>
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
      </svg>
      <!-- Stars -->
      <div class="stars">
        <span class="star" style="--delay: 0.1s; --x: 5px; --y: -8px">✦</span>
        <span class="star" style="--delay: 0.2s; --x: -8px; --y: 5px">✦</span>
        <span class="star" style="--delay: 0.3s; --x: 10px; --y: 8px">✦</span>
      </div>
    </div>
    
    <!-- Slider indicator -->
    <div class="slider"></div>
  </div>
  
  <span class="toggle-label">
    {theme === 'light' ? 'Day' : 'Night'}
  </span>
</button>

<style>
  .theme-toggle {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
    padding: var(--space-xs);
    background: var(--bg-secondary);
    border: 2px solid var(--bg-tertiary);
    border-radius: 30px;
    cursor: pointer;
    transition: all var(--transition-normal);
  }
  
  .theme-toggle:hover {
    border-color: var(--accent-primary);
  }
  
  .theme-toggle.animating {
    pointer-events: none;
  }
  
  .toggle-track {
    position: relative;
    width: 60px;
    height: 30px;
    border-radius: 15px;
    background: linear-gradient(135deg, #87CEEB 0%, #FDB813 100%);
    transition: background var(--transition-slow);
    overflow: hidden;
  }
  
  .theme-toggle.dark .toggle-track {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  }
  
  .sun, .moon {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 20px;
    height: 20px;
    transition: all var(--transition-normal);
    opacity: 0.3;
  }
  
  .sun {
    left: 6px;
    color: #FDB813;
  }
  
  .sun.active {
    opacity: 1;
    animation: sunRise 0.5s ease;
  }
  
  .moon {
    right: 6px;
    color: #f0f0f0;
  }
  
  .moon.active {
    opacity: 1;
    animation: moonRise 0.5s ease;
  }
  
  .sun svg, .moon svg {
    width: 100%;
    height: 100%;
  }
  
  .stars {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }
  
  .star {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(var(--x), var(--y));
    font-size: 6px;
    color: #fff;
    opacity: 0;
    animation: twinkle 1.5s ease-in-out infinite;
    animation-delay: var(--delay);
  }
  
  .moon.active .star {
    opacity: 1;
  }
  
  .slider {
    position: absolute;
    top: 3px;
    left: 3px;
    width: 24px;
    height: 24px;
    border-radius: var(--radius-full);
    background: white;
    box-shadow: var(--shadow-sm);
    transition: transform var(--transition-normal);
  }
  
  .theme-toggle.dark .slider {
    transform: translateX(30px);
    background: #2d3436;
  }
  
  .toggle-label {
    font-family: var(--font-mono);
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-secondary);
    min-width: 35px;
    padding-right: var(--space-sm);
  }
  
  @keyframes sunRise {
    0% { transform: translateY(-50%) scale(0.5) rotate(-180deg); }
    100% { transform: translateY(-50%) scale(1) rotate(0deg); }
  }
  
  @keyframes moonRise {
    0% { transform: translateY(-50%) scale(0.5) rotate(180deg); }
    100% { transform: translateY(-50%) scale(1) rotate(0deg); }
  }
  
  @keyframes twinkle {
    0%, 100% { opacity: 0.3; transform: translate(var(--x), var(--y)) scale(0.8); }
    50% { opacity: 1; transform: translate(var(--x), var(--y)) scale(1.2); }
  }
</style>
