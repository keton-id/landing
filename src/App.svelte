<script>
  const links = [
    { label: 'GitHub', href: 'https://github.com/keton-id' },
    { label: 'Projects', href: '#projects' },
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
      desc: 'A Jira toolkit for terminals, coding assistants, and bots.',
      href: 'https://jirac.keton.id'
    },
    {
      name: 'vod',
      type: 'Dashboard',
      desc: 'A virtual office dashboard to monitor multiple Google Meet rooms from one place.',
      href: 'https://github.com/keton-id/vod'
    },
    {
      name: 'next build',
      type: 'In Progress',
      desc: 'Always cooking something useful, weird, or quietly powerful.',
      href: 'https://github.com/keton-id'
    }
  ]
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
    <nav>
      {#each links as link}
        <a href={link.href}>{link.label}</a>
      {/each}
    </nav>
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
            <p>{project.desc}</p>
          </a>
        {/each}
      </div>
    </section>
  </main>
</div>
