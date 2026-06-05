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

  const stackIcons = {
    Rust: { slug: 'rust', color: 'CE412B' },
    Shell: { slug: 'gnubash', color: '4EAA25' },
    Zig: { slug: 'zig', color: 'F7A41D' },
    JavaScript: { slug: 'javascript', color: 'F7DF1E' },
    TypeScript: { slug: 'typescript', color: '3178C6' },
    Swift: { slug: 'swift', color: 'F05138' },
    Docker: { slug: 'docker', color: '2496ED' },
    Ruby: { slug: 'ruby', color: 'CC342D' }
  }

  const projects = [
    {
      name: 'jirac',
      mark: 'jc',
      markVariant: 'jirac',
      type: 'CLI / TUI / MCP',
      stack: ['Rust', 'Shell'],
      desc: 'A fast Jira CLI with terminal UI and MCP integration for AI-assisted workflows.',
      href: 'https://jirac.keton.id'
    },
    {
      name: 'cora',
      mark: '🤫',
      markVariant: 'cora',
      type: 'Secrets / AI Agents',
      stack: ['Zig'],
      desc: 'Zero-knowledge secret injection for AI agents. One encrypted file, one passphrase, no secrets in env.',
      href: 'https://cora.keton.id'
    },
    {
      name: 'pkgmap',
      mark: 'pk',
      markVariant: 'default',
      type: 'Monitoring Tool',
      stack: ['JavaScript', 'Shell'],
      desc: 'A package-state and dependency mapping tool for understanding what is installed and why.',
      href: 'https://github.com/mulhamna/pkgmap'
    }
  ]

  let theme = 'dark'

  onMount(() => {
    const saved = localStorage.getItem('keton-theme')
    const prefersLight = window.matchMedia('(prefers-color-scheme: light)').matches
    theme = saved || (prefersLight ? 'light' : 'dark')
    document.documentElement.setAttribute('data-theme', theme)

    const io = new IntersectionObserver(
      (entries) => {
        for (const e of entries) {
          if (e.isIntersecting) {
            e.target.classList.add('is-revealed')
            io.unobserve(e.target)
          }
        }
      },
      { threshold: 0.12 }
    )
    document.querySelectorAll('[data-reveal]').forEach((el) => io.observe(el))
  })

  function toggleTheme() {
    theme = theme === 'dark' ? 'light' : 'dark'
    document.documentElement.setAttribute('data-theme', theme)
    localStorage.setItem('keton-theme', theme)
  }
</script>

<svelte:head>
  <title>keton.id — always cooking something</title>
  <meta
    name="description"
    content="keton.id builds developer-flavored tools, internal systems, and always seems to be cooking something new."
  />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link
    href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=JetBrains+Mono:wght@400;500;600;700&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<div class="page-shell">
  <header class="topbar">
    <a class="brand" href="/" aria-label="keton.id home">
      <img class="brand__mark" src="/favicon.svg" alt="" width="36" height="36" />
      <span class="brand__name">keton<span class="brand__dot">.</span>id</span>
    </a>

    <div class="topbar-actions">
      <nav>
        {#each links as link}
          <a href={link.href}>{link.label}</a>
        {/each}
      </nav>

      <button class="theme-toggle" type="button" on:click={toggleTheme} aria-label="Toggle color mode">
        {theme === 'dark' ? 'Light' : 'Dark'}
      </button>
    </div>
  </header>

  <main>
    <section class="hero" data-reveal>
      <div class="hero__glow" aria-hidden="true"></div>
      <div class="hero__grid" aria-hidden="true"></div>
      <svg class="hero__holo" viewBox="0 0 1440 860" preserveAspectRatio="xMidYMid slice" aria-hidden="true" focusable="false">
        <defs>
          <linearGradient id="ketonHolo" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0" stop-color="var(--accent)" />
            <stop offset="0.5" stop-color="var(--accent-2)" />
            <stop offset="1" stop-color="var(--accent)" />
          </linearGradient>
          <g id="ketonOrbit">
            <circle r="120" fill="none" stroke="url(#ketonHolo)" stroke-width="1.2" />
            <circle r="78" fill="none" stroke="url(#ketonHolo)" stroke-width="0.8" />
            <circle r="36" fill="none" stroke="url(#ketonHolo)" stroke-width="0.6" />
            <circle cx="120" cy="0" r="3" fill="url(#ketonHolo)" />
            <circle cx="-78" cy="0" r="2" fill="url(#ketonHolo)" />
          </g>
        </defs>
        <use href="#ketonOrbit" transform="translate(180 200) rotate(15) scale(1.4)" opacity="0.18" />
        <use href="#ketonOrbit" transform="translate(1260 280) rotate(-22) scale(1.9)" opacity="0.14" />
        <use href="#ketonOrbit" transform="translate(820 760) rotate(8) scale(2.2)" opacity="0.08" />
      </svg>

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
          <span class="terminal-bar__title">keton.id — zsh</span>
        </div>
        <div class="terminal-body">
          <p><span class="prompt">$</span> whoami</p>
          <p class="answer">keton.id</p>
          <p><span class="prompt">$</span> status</p>
          <p class="answer">shipping tools for people who build</p>
          <p><span class="prompt">$</span> stack</p>
          <p class="answer">rust · zig · swift · typescript</p>
          <p><span class="prompt">$</span> motto</p>
          <p class="answer accent caret">always cooking something</p>
        </div>
      </aside>
    </section>

    <section class="signal-grid" id="stack" data-reveal>
      <article>
        <span class="dot" aria-hidden="true"></span>
        <p class="label">Mode</p>
        <h2>Developer-first</h2>
        <p>Built with product sense, implementation detail, and a love for useful interfaces.</p>
      </article>
      <article>
        <span class="dot" aria-hidden="true"></span>
        <p class="label">Style</p>
        <h2>Clean, fast, responsive</h2>
        <p>Designed to feel sharp on mobile first, but still calm and polished on desktop.</p>
      </article>
      <article>
        <span class="dot" aria-hidden="true"></span>
        <p class="label">Energy</p>
        <h2>Quietly shipping</h2>
        <p>Not loud for the sake of it, just consistently making things that matter.</p>
      </article>
    </section>

    <section class="projects" id="projects" data-reveal>
      <div class="section-head">
        <p class="eyebrow">selected work</p>
        <h2>Things on the stove</h2>
      </div>

      <div class="project-grid">
        {#each projects as project}
          <a class="project-card" href={project.href} target="_blank" rel="noreferrer">
            <div class="project-top">
              <span class="project-mark project-mark--{project.markVariant}">{project.mark}</span>
              <span class="arrow" aria-hidden="true">↗</span>
            </div>
            <h3>{project.name}</h3>
            <p class="type">{project.type}</p>
            <div class="project-divider" aria-hidden="true"></div>
            <p class="project-desc">{project.desc}</p>
            <div class="project-stack" aria-label={`${project.name} stack`}>
              {#each project.stack as item}
                <span>
                  {#if stackIcons[item]}
                    <img
                      class="stack-icon"
                      src={`https://cdn.simpleicons.org/${stackIcons[item].slug}/${stackIcons[item].color}`}
                      alt=""
                      width="14"
                      height="14"
                      loading="lazy"
                    />
                  {/if}
                  {item}
                </span>
              {/each}
            </div>
          </a>
        {/each}
      </div>
    </section>
  </main>

  <footer class="site-footer" data-reveal>
    <p>© {new Date().getFullYear()} keton.id — always cooking something.</p>
    <a href="mailto:hello@keton.id">hello@keton.id</a>
  </footer>
</div>
