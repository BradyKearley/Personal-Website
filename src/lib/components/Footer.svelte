<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  
  let now = new Date();
  let intervalId: number;
  
  $: timeString = now.toLocaleTimeString([], { 
    hour: '2-digit', 
    minute: '2-digit', 
    second: '2-digit',
    hour12: true 
  });
  
  $: dateString = now.toLocaleDateString([], {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
  
  onMount(() => {
    intervalId = setInterval(() => {
      now = new Date();
    }, 1000);
  });
  
  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

<footer class="footer">
  <div class="footer-content">
    <!-- Left: Branding -->
    <div class="footer-brand">
      <div class="brand-logo">⏰</div>
      <div class="brand-text">
        <span class="brand-name">Brady's Portfolio</span>
        <span class="brand-tagline">Time well spent coding</span>
      </div>
    </div>
    
    <!-- Center: Real-time clock -->
    <div class="footer-clock">
      <div class="clock-display">
        <time datetime={now.toISOString()} class="time">
          {timeString}
        </time>
        <span class="date">{dateString}</span>
      </div>
      <div class="clock-decoration">
        <span class="tick-animation">⚡</span>
      </div>
    </div>
    
    <!-- Right: Links -->
    <div class="footer-links">
      <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="footer-link">
        <span class="link-icon">💻</span>
        <span>GitHub</span>
      </a>
      <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="footer-link">
        <span class="link-icon">💼</span>
        <span>LinkedIn</span>
      </a>
      <a href="mailto:contact@example.com" class="footer-link">
        <span class="link-icon">✉️</span>
        <span>Email</span>
      </a>
    </div>
  </div>
  
  <!-- Bottom bar -->
  <div class="footer-bottom">
    <p>© {now.getFullYear()} — Built with Svelte & lots of ☕</p>
    <p class="footer-quote">"Time is the most valuable thing a man can spend." — Theophrastus</p>
  </div>
</footer>

<style>
  .footer {
    background: var(--bg-secondary);
    border-top: 1px solid var(--bg-tertiary);
    padding: var(--space-xl) 0 var(--space-lg);
    margin-top: var(--space-2xl);
  }
  
  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 var(--space-lg);
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    gap: var(--space-xl);
    align-items: center;
  }
  
  /* Brand */
  .footer-brand {
    display: flex;
    align-items: center;
    gap: var(--space-md);
  }
  
  .brand-logo {
    font-size: 2.5rem;
    animation: tick 2s ease-in-out infinite;
  }
  
  .brand-text {
    display: flex;
    flex-direction: column;
  }
  
  .brand-name {
    font-family: var(--font-display);
    font-size: 1.2rem;
    font-weight: 600;
    color: var(--text-primary);
  }
  
  .brand-tagline {
    font-size: 0.85rem;
    color: var(--text-secondary);
    font-style: italic;
  }
  
  /* Clock */
  .footer-clock {
    display: flex;
    align-items: center;
    gap: var(--space-md);
    background: var(--bg-tertiary);
    padding: var(--space-md) var(--space-lg);
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-sm);
  }
  
  .clock-display {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .time {
    font-family: var(--font-mono);
    font-size: 1.8rem;
    font-weight: 700;
    color: var(--accent-primary);
    letter-spacing: 2px;
  }
  
  .date {
    font-size: 0.8rem;
    color: var(--text-secondary);
    margin-top: 2px;
  }
  
  .clock-decoration {
    display: flex;
    align-items: center;
  }
  
  .tick-animation {
    font-size: 1.2rem;
    animation: pulse 1s ease-in-out infinite;
  }
  
  /* Links */
  .footer-links {
    display: flex;
    gap: var(--space-md);
    justify-content: flex-end;
  }
  
  .footer-link {
    display: flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-sm) var(--space-md);
    background: var(--bg-tertiary);
    border-radius: var(--radius-md);
    color: var(--text-primary);
    text-decoration: none;
    font-size: 0.9rem;
    transition: all var(--transition-fast);
  }
  
  .footer-link:hover {
    background: var(--accent-primary);
    color: white;
    transform: translateY(-2px);
  }
  
  .link-icon {
    font-size: 1.1rem;
  }
  
  /* Bottom */
  .footer-bottom {
    max-width: 1200px;
    margin: var(--space-xl) auto 0;
    padding: var(--space-md) var(--space-lg) 0;
    border-top: 1px solid var(--bg-tertiary);
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.85rem;
    color: var(--text-secondary);
  }
  
  .footer-quote {
    font-style: italic;
    opacity: 0.7;
  }
  
  /* Responsive */
  @media (max-width: 900px) {
    .footer-content {
      grid-template-columns: 1fr;
      text-align: center;
      gap: var(--space-lg);
    }
    
    .footer-brand {
      justify-content: center;
    }
    
    .footer-links {
      justify-content: center;
    }
    
    .footer-bottom {
      flex-direction: column;
      gap: var(--space-sm);
      text-align: center;
    }
  }
  
  @media (max-width: 500px) {
    .footer-links {
      flex-direction: column;
      align-items: center;
    }
    
    .footer-link span:not(.link-icon) {
      display: none;
    }
    
    .footer-links {
      flex-direction: row;
    }
  }
</style>
