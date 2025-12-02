<script lang="ts">
  import { slide } from "svelte/transition";
  import LiveTime from "$lib/components/LiveTime.svelte";
  import Countdown from "$lib/components/Countdown.svelte";

  const birthdayDate = new Date("2005-04-15T14:35:00");
  const graduationDate = new Date("2028-04-30");
  const coopStartDate = new Date("2026-05-01");

  const skills = [
    "Godot",
    "Flutter",
    "E2E Testing",
    "Python",
    "Unity ML-Agents",
    "Git",
    "Cypress",
  ];

  const projects = [
    {
      name: "Boop Bot Keybound",
      date: new Date("2025-01-18"),
      desc: "1st place CuHacking 2025 - Puzzle platformer",
      details:
        "Developed an innovative puzzle platformer featuring CuHacking's logo as the playable character. Built custom level design mechanics and physics systems in Godot, earning first place overall at CuHacking 2025.",
      github: "https://github.com/BradyKearley/CuHacking",
      tags: ["Godot", "Game Dev"],
    },
    {
      name: "Self Driving Race Car",
      date: new Date("2025-01-10"),
      desc: "ML car using PPO reinforcement learning",
      details:
        "Created an AI model to autonomously drive a car around a track using reinforcement learning. Implemented the PPO (Proximal Policy Optimization) algorithm with Unity ML-Agents and CUDA acceleration for training.",
      github: "https://github.com/BradyKearley/Self-Driving-Car-ML",
      tags: ["Unity", "ML-Agents"],
    },
    {
      name: "Plants Vs Pesticides",
      date: new Date("2024-12-01"),
      desc: "1st place UI/UX - uOttawa Hack",
      details:
        "Won first place in the UI/UX challenge at uOttawa Hack 7. Created an engaging game with custom visual and auditory elements, focusing on intuitive user experience and polished interface design.",
      github: "https://github.com/BradyKearley/Plants-Vs-Pesticides",
      tags: ["Game Dev", "UI/UX"],
    },
    {
      name: "Steganography Encryption",
      date: new Date("2024-10-15"),
      desc: "RSA encryption + image steganography",
      details:
        "Produced government-level RSA encryption and implemented steganography techniques for secure data transfer. Messages are encrypted then hidden within image files, making the data invisible to casual observation.",
      github: "https://github.com/BradyKearley/Steganography",
      tags: ["Python", "Encryption", "C"],
    },
    {
      name: "Carleton 360+",
      date: new Date("2024-09-28"),
      desc: "Hack the Tunnels - Enhanced search UI",
      details:
        "Enhanced user experience with improved search capabilities using Prisma database and redesigned UI/UX for key pages. Built during Hack the Tunnels hackathon at Carleton University.",
      github: "https://github.com/ChrisPham03/hack_the_tunnel",
      tags: ["React", "Prisma"],
    },
    {
      name: "Nurture",
      date: new Date("2024-01-20"),
      desc: "Hack the Hills - Firebase auth + analytics",
      details:
        "Implemented secure backend with Firebase authentication and Google Analytics integration. Designed seamless UI/UX with a natural theme for this wellness-focused application.",
      github: "https://github.com/miguelclosa-ca/hth24-nurture",
      tags: ["React", "Firebase"],
    },
    {
      name: "CS50 Final Project",
      date: new Date("2023-12-01"),
      desc: "Harvard CS50 - 2D game with custom sprites",
      details:
        "Created object-based entities using nodes and GDScript for Harvard's CS50 course. Designed custom 2D sprites and original music for a complete gaming experience.",
      github: "https://github.com/BradyKearley/CS50-Final-Project",
      tags: ["Godot", "GDScript"],
    },
  ];

  let expandedProject: string | null = $state(null);
  let expandedExperience: string | null = $state(null);

  function toggleProject(name: string) {
    expandedProject = expandedProject === name ? null : name;
  }

  function toggleExperience(role: string) {
    expandedExperience = expandedExperience === role ? null : role;
  }

  const experience = [
    {
      role: "QA Co-op",
      company: "Giatec",
      period: "May 2025 - Dec 2025",
      desc: "E2E testing with Flutter & Cypress",
      details:
        "Developed and maintained end-to-end test suites using Flutter integration tests and Cypress for web applications. Automated regression testing, identified bugs, and collaborated with developers to ensure software quality across multiple product releases.",
    },
    {
      role: "Laydown Operator (Mixing)",
      company: "Goodyear",
      period: "May 2024 - Sep 2024",
      desc: "Manufacturing operations",
      details:
        "Operated mixing equipment in tire manufacturing facility. Maintained quality standards, followed safety protocols, and worked as part of a team in a fast-paced industrial environment.",
    },
  ];

  const coursework = [
    "Data Structures & Algorithms",
    "Object-Oriented Programming",
    "Discrete Mathematics",
    "Calculus",
    "Java Programming",
  ];

  let theme = $state("dark");

  function toggleTheme() {
    theme = theme === "dark" ? "light" : "dark";
    document.documentElement.setAttribute("data-theme", theme);
  }
</script>

<svelte:head>
  <title>Brady | Developer</title>
</svelte:head>

<main class="page">
  <!-- Header -->
  <header class="header">
    <LiveTime size="sm" showDate={true} />
    <button class="theme-btn" onclick={toggleTheme} aria-label="Toggle theme">
      {theme === "dark" ? "○" : "●"}
    </button>
  </header>

  <!-- Hero -->
  <section class="hero">
    <p class="time-label">alive for</p>
    <div class="hero-time">
      <Countdown targetDate={birthdayDate} label="" size="xl" />
    </div>
    <h1>Brady Kearley</h1>
    <p class="tagline">Computer Science Student</p>
  </section>

  <hr class="divider" />

  <!-- About -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// about</span>
    </div>
    <p>
      Building games, ML systems, and mobile apps. Hackathon winner. Currently
      pursuing CS Honours with Co-Op at Carleton University.
    </p>
  </section>

  <hr class="divider" />

  <!-- Experience -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// experience</span>
    </div>
    <ul class="experience-list">
      {#each experience as job}
        <li
          class="experience-item"
          class:expanded={expandedExperience === job.role}
        >
          <button
            class="experience-toggle"
            onclick={() => toggleExperience(job.role)}
          >
            <div class="experience-main">
              <span class="experience-role">{job.role}</span>
              <span class="experience-company">{job.company}</span>
            </div>
            <div class="experience-right">
              <span class="experience-period">{job.period}</span>
              <span class="expand-icon"
                >{expandedExperience === job.role ? "−" : "+"}</span
              >
            </div>
          </button>
          {#if expandedExperience === job.role}
            <div
              class="experience-details"
              transition:slide={{ duration: 200 }}
            >
              <p>{job.details}</p>
            </div>
          {/if}
        </li>
      {/each}
    </ul>
  </section>

  <hr class="divider" />

  <!-- Education -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// education</span>
    </div>

    <div class="info-grid">
      <div class="info-item">
        <span class="info-label">degree</span>
        <span class="info-value">B.Sc. Computer Science</span>
      </div>
      <div class="info-item">
        <span class="info-label">program</span>
        <span class="info-value">Honours with Co-Op</span>
      </div>
      <div class="info-item">
        <span class="info-label">school</span>
        <span class="info-value">Carleton University</span>
      </div>
      <div class="info-item">
        <span class="info-label">cgpa</span>
        <span class="info-value highlight">11.33 / 12.00</span>
      </div>
    </div>

    <div class="countdown-row">
      <Countdown targetDate={graduationDate} label="graduation" />
    </div>

    <div class="list-section">
      <span class="info-label">coursework</span>
      <ul class="simple-list">
        {#each coursework as course}
          <li>{course}</li>
        {/each}
      </ul>
    </div>
  </section>

  <hr class="divider" />

  <!-- Co-op -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// co-op</span>
      <span class="status available">available</span>
    </div>

    <div class="info-grid">
      <div class="info-item">
        <span class="info-label">start</span>
        <span class="info-value">Summer 2026</span>
      </div>
      <div class="info-item">
        <span class="info-label">duration</span>
        <span class="info-value">4 or 8 months</span>
      </div>
    </div>

    <div class="countdown-row">
      <Countdown targetDate={coopStartDate} label="availability" />
    </div>
  </section>

  <hr class="divider" />

  <!-- Skills -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// skills</span>
    </div>
    <div class="tags">
      {#each skills as skill}
        <span class="tag">{skill}</span>
      {/each}
    </div>
  </section>

  <hr class="divider" />

  <!-- Projects -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// projects</span>
    </div>
    <ul class="project-list">
      {#each projects as project}
        <li
          class="project-item"
          class:expanded={expandedProject === project.name}
        >
          <button
            class="project-toggle"
            onclick={() => toggleProject(project.name)}
          >
            <div class="project-main">
              <span class="project-name">{project.name}</span>
              <span class="project-desc">{project.desc}</span>
            </div>
            <div class="project-right">
              <Countdown targetDate={project.date} size="sm" />
              <span class="expand-icon"
                >{expandedProject === project.name ? "−" : "+"}</span
              >
            </div>
          </button>
          {#if expandedProject === project.name}
            <div class="project-details" transition:slide={{ duration: 200 }}>
              <p>{project.details}</p>
              <a
                href={project.github}
                target="_blank"
                rel="noopener"
                class="project-link"
              >
                → View on GitHub
              </a>
            </div>
          {/if}
        </li>
      {/each}
    </ul>
  </section>

  <hr class="divider" />

  <!-- Contact -->
  <section class="section">
    <div class="section-header">
      <span class="section-label">// contact</span>
    </div>
    <div class="contact-grid">
      <div class="contact-item">
        <span class="info-label">email</span>
        <a href="mailto:bradykearley84@gmail.com">bradykearley84@gmail.com</a>
      </div>
      <div class="contact-item">
        <span class="info-label">phone</span>
        <a href="tel:613-328-2858">613-328-2858</a>
      </div>
      <div class="contact-item">
        <span class="info-label">location</span>
        <span>Ontario, Canada</span>
      </div>
    </div>
    <div class="links">
      <a href="https://github.com/BradyKearley" target="_blank" rel="noopener"
        >github</a
      >
      <span class="link-sep">·</span>
      <a href="https://linkedin.com/in/kearleyb" target="_blank" rel="noopener"
        >linkedin</a
      >
      <span class="link-sep">·</span>
      <a
        href="https://www.bradykearley.me/Resume-Brady%20Kearley.pdf"
        target="_blank"
        rel="noopener">resume</a
      >
    </div>
  </section>

  <!-- Footer -->
  <footer class="footer">
    <LiveTime size="sm" showDate={true} showMilliseconds={true} />
  </footer>
</main>

<style>
  .page {
    max-width: 900px;
    margin: 0 auto;
    padding: var(--space-lg) var(--space-xl);
    min-height: 100vh;
  }

  /* Header */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: var(--space-xl);
    padding-bottom: var(--space-md);
    border-bottom: 1px solid var(--border);
  }

  .theme-btn {
    background: none;
    border: 1px solid var(--border);
    color: var(--text);
    font-size: 1rem;
    width: 2rem;
    height: 2rem;
    border-radius: 50%;
    cursor: pointer;
    transition: border-color 0.2s;
  }

  .theme-btn:hover {
    border-color: var(--accent);
  }

  /* Hero */
  .hero {
    text-align: center;
    padding: var(--space-xl) 0;
  }

  .hero-time {
    margin-bottom: var(--space-lg);
  }

  .time-label {
    color: var(--text-muted);
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: var(--space-sm);
  }

  .hero h1 {
    font-size: 3rem;
    font-weight: 400;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: var(--space-sm);
  }

  .tagline {
    color: var(--text-muted);
    font-size: 1rem;
  }

  /* Sections */
  .section {
    padding: var(--space-lg) 0;
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: var(--space-md);
    margin-bottom: var(--space-md);
  }

  .section-label {
    color: var(--text-muted);
    font-size: 0.875rem;
  }

  .status {
    font-size: 0.75rem;
    padding: 0.25em 0.75em;
    border-radius: 2px;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .status.available {
    background: var(--accent);
    color: var(--bg);
  }

  /* Info grid */
  .info-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: var(--space-md) var(--space-lg);
    margin-bottom: var(--space-md);
  }

  .info-item {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .info-label {
    color: var(--text-muted);
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .info-value {
    color: var(--text);
  }

  .info-value.highlight {
    color: var(--accent);
  }

  .countdown-row {
    margin: var(--space-md) 0;
    padding: var(--space-md);
    background: var(--bg-subtle);
    border-left: 2px solid var(--accent);
  }

  /* Lists */
  .list-section {
    margin-top: var(--space-md);
  }

  .simple-list {
    list-style: none;
    margin-top: var(--space-sm);
  }

  .simple-list li {
    padding: var(--space-xs) 0;
    color: var(--text-muted);
  }

  .simple-list li::before {
    content: "→ ";
    color: var(--accent);
  }

  /* Tags */
  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-sm);
  }

  .tag {
    padding: var(--space-xs) var(--space-sm);
    border: 1px solid var(--border);
    font-size: 0.875rem;
    color: var(--text-muted);
  }

  /* Experience */
  .experience-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: var(--space-sm);
  }

  .experience-item {
    border: 1px solid var(--border);
    transition: border-color 0.2s;
  }

  .experience-item:hover {
    border-color: var(--text-muted);
  }

  .experience-item.expanded {
    border-color: var(--accent);
  }

  .experience-toggle {
    background: none;
    border: none;
    padding: var(--space-md);
    cursor: pointer;
    text-align: left;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: var(--space-md);
    color: inherit;
    font-family: inherit;
  }

  .experience-main {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
  }

  .experience-right {
    display: flex;
    align-items: center;
    gap: var(--space-md);
  }

  .experience-right .expand-icon {
    position: static;
    color: var(--text-muted);
    font-size: 1.25rem;
    line-height: 1;
  }

  .experience-role {
    color: var(--text);
  }

  .experience-company {
    color: var(--accent);
    font-size: 0.875rem;
  }

  .experience-company::before {
    content: "@ ";
    color: var(--text-muted);
  }

  .experience-period {
    color: var(--text-muted);
    font-size: 0.75rem;
  }

  .experience-details {
    padding: 0 var(--space-md) var(--space-md);
    border-top: 1px solid var(--border);
  }

  .experience-details p {
    color: var(--text-muted);
    font-size: 0.875rem;
    line-height: 1.6;
    margin: var(--space-md) 0 0;
  }

  /* Projects */
  .project-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: var(--space-sm);
  }

  .project-item {
    border: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    transition: border-color 0.2s;
  }

  .project-item:hover {
    border-color: var(--text-muted);
  }

  .project-item.expanded {
    border-color: var(--accent);
  }

  .project-toggle {
    background: none;
    border: none;
    padding: var(--space-md);
    cursor: pointer;
    text-align: left;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: var(--space-md);
    color: inherit;
    font-family: inherit;
  }

  .project-main {
    display: flex;
    align-items: center;
    gap: var(--space-sm);
  }

  .project-right {
    display: flex;
    align-items: center;
    gap: var(--space-md);
    flex-shrink: 0;
  }

  .project-right .expand-icon {
    position: static;
    color: var(--text-muted);
    font-size: 1.25rem;
    line-height: 1;
  }

  .project-name {
    color: var(--text);
    font-weight: 500;
  }

  .project-item:hover .project-name {
    color: var(--accent);
  }

  .project-desc {
    color: var(--text-muted);
    font-size: 0.875rem;
  }

  .project-desc::before {
    content: "— ";
    color: var(--text-muted);
  }

  .project-details {
    padding: 0 var(--space-md) var(--space-md);
    border-top: 1px solid var(--border);
    margin-top: 0;
  }

  .project-details p {
    color: var(--text-muted);
    font-size: 0.875rem;
    line-height: 1.6;
    margin: var(--space-md) 0;
  }

  .project-link {
    display: inline-block;
    font-size: 0.875rem;
    color: var(--accent);
  }

  .project-link:hover {
    text-decoration: underline;
  }

  /* Contact */
  .contact-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--space-md);
    margin-bottom: var(--space-md);
  }

  .contact-item {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .contact-item a {
    font-size: 0.875rem;
  }

  /* Links */
  .links {
    display: flex;
    gap: var(--space-sm);
    align-items: center;
    margin-top: var(--space-md);
  }

  .link-sep {
    color: var(--text-muted);
  }

  /* Divider */
  .divider {
    border: none;
    border-top: 1px solid var(--border);
    margin: 0;
  }

  /* Footer */
  .footer {
    margin-top: var(--space-xl);
    padding-top: var(--space-lg);
    border-top: 1px solid var(--border);
    text-align: center;
  }

  /* Responsive */
  @media (max-width: 800px) {
    .info-grid {
      grid-template-columns: repeat(2, 1fr);
    }

    .project-list {
      grid-template-columns: 1fr;
    }
  }

  @media (max-width: 600px) {
    .info-grid {
      grid-template-columns: 1fr;
    }

    .contact-grid {
      grid-template-columns: 1fr;
    }

    .experience-item {
      grid-template-columns: 1fr;
      gap: var(--space-xs);
    }

    .hero h1 {
      font-size: 2rem;
    }
  }
</style>
