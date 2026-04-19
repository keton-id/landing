<script>
  import { onMount } from 'svelte'

  const links = [
    { label: 'GitHub', href: 'https://github.com/keton-id' },
    { label: 'Projects', href: '#projects' },
    { label: 'Stack', href: '#stack' },
    { label: 'Contact', href: 'mailto:hello@keton.id' }
  ]

  const highlights = [
    'developer-first tools',
    'practical internal systems',
    'small experiments that ship'
  ]

  const projects = [
    {
      name: 'jira-commands',
      type: 'CLI / Agent Tooling',
      stack: ['Rust', 'Shell'],
      desc: 'A Jira toolkit for terminals, coding assistants, and bots.',
      href: 'https://jirac.keton.id'
    },
    {
      name: 'vod',
      type: 'Dashboard',
      stack: ['TypeScript', 'JavaScript', 'Docker'],
      desc: 'A virtual office dashboard to monitor multiple Google Meet rooms from one place.',
      href: 'https://github.com/keton-id/vod'
    },
    {
      name: 'pkgmap',
      type: 'Monitoring Tool',
      stack: ['JavaScript', 'Shell'],
      desc: 'A package-state and dependency mapping tool for understanding what is installed and why.',
      href: 'https://github.com/mulhamna/pkgmap'
    },
    {
      name: 'portbar',
      type: 'Mac Utility',
      stack: ['Swift', 'Shell', 'Ruby'],
      desc: 'A lightweight menu bar utility for monitoring ports and local service exposure.',
      href: 'https://github.com/mulhamna/portbar'
    }
  ]

  let theme = 'dark'

  onMount(() => {
    const saved = localStorage.getItem('keton-theme')
    const prefersLight = window.matchMedia('(prefers-color-scheme: light)').matches
    theme = saved || (prefersLight ? 'light' : 'dark')
    document.documentElement.setAttribute('data-theme', theme)
  })

  function toggleTheme() {
    theme = theme === 'dark' ? 'light' : 'dark'
    document.documentElement.setAttribute('data-theme', theme)
    localStorage.setItem('keton-theme', theme)
  }
</script>

<svelte:head>
  <title>keton.id</title>
  <meta
    name="description"
    content="keton.id builds developer-flavored tools, internal systems, and always seems to be cooking something new."
  />
</svelte:head>

<div class="page-shell">
  <header class="topbar">
    <a class="brand" href="/">keton.id</a>

    <div class="topbar-actions">
      <nav>
        {#each links as link}
          <a href={link.href}>{link.label}</a>
        {/each}
      </nav>

      <button class="theme-toggle" type="button" on:click={toggleTheme} aria-label="Toggle color mode">
        {theme === 'dark' ? 'Light mode' : 'Dark mode'}
      </button>
    </div>
  </header>

  <main>
    <section class="hero">
      <div class="hero-copy">
        <p class="eyebrow">developer nuance, practical output</p>
        <h1>Always cooking something.</h1>
        <p class="lede">
          Keton builds developer-facing tools and internal systems with a bias for shipping,
          clarity, and useful little edges.
        </p>

        <div class="chip-row" aria-label="Highlights">
          {#each highlights as item}
            <span>{item}</span>
          {/each}
        </div>

        <div class="cta-row">
          <a class="btn primary" href="#projects">See what&apos;s cooking</a>
          <a class="btn secondary" href="https://github.com/keton-id">Open GitHub</a>
        </div>
      </div>

      <aside class="terminal-card" aria-label="Developer preview card">
        <div class="terminal-bar">
          <span></span><span></span><span></span>
        </div>
        <div class="terminal-body">
          <p><span class="prompt">$</span> whoami</p>
          <p class="answer">keton.id</p>
          <p><span class="prompt">$</span> status</p>
          <p class="answer">shipping tools for people who build</p>
          <p><span class="prompt">$</span> motto</p>
          <p class="answer accent">always cooking something</p>
        </div>
      </aside>
    </section>

    <section class="signal-grid">
      <article>
        <p class="label">Mode</p>
        <h2>Developer-first</h2>
        <p>Built with product sense, implementation detail, and a love for useful interfaces.</p>
      </article>
      <article>
        <p class="label">Style</p>
        <h2>Clean, fast, responsive</h2>
        <p>Designed to feel sharp on mobile first, but still calm and polished on desktop.</p>
      </article>
      <article>
        <p class="label">Energy</p>
        <h2>Quietly shipping</h2>
        <p>Not loud for the sake of it, just consistently making things that matter.</p>
      </article>
    </section>

    <section class="projects" id="projects">
      <div class="section-head">
        <p class="eyebrow">selected work</p>
        <h2>Things on the stove</h2>
      </div>

      <div class="project-grid">
        {#each projects as project}
          <a class="project-card" href={project.href} target="_blank" rel="noreferrer">
            <div class="project-top">
              <span class="type">{project.type}</span>
              <span class="arrow">↗</span>
            </div>
            <h3>{project.name}</h3>
            <div class="project-stack" aria-label={`${project.name} stack`}>
              {#each project.stack as item}
                <span>{item}</span>
              {/each}
            </div>
            <p>{project.desc}</p>
          </a>
        {/each}
      </div>
    </section>
  </main>
</div>
