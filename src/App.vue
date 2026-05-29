<script setup lang="ts">
import {
  ArrowUpRight,
  BadgeCheck,
  BriefcaseBusiness,
  CalendarDays,
  CheckCircle2,
  Cloud,
  Code2,
  Database,
  Github,
  Linkedin,
  Mail,
  MapPin,
  Menu,
  Phone,
  ServerCog,
  ShieldCheck,
  Sparkles,
  X,
} from 'lucide-vue-next'
import { onMounted, onUnmounted, ref } from 'vue'

type Project = {
  title: string
  eyebrow: string
  summary: string
  image?: string
  link?: string
  impact: string[]
  stack: string[]
}

const isMenuOpen = ref(false)
const activeSection = ref('home')
const missingImages = ref(new Set<string>())
const currentYear = new Date().getFullYear()
const logoImage = '/portfolio/logo.png'
const profileImage = '/portfolio/profile.jpg'

const navItems = [
  { label: 'Home', href: '#home' },
  { label: 'About', href: '#about' },
  { label: 'Projects', href: '#projects' },
  { label: 'Experience', href: '#experience' },
  { label: 'Contact', href: '#contact' },
]

const stats = [
  { value: '7+', label: 'Years building production systems' },
  { value: '5', label: 'Years with Python, Django, and Flask' },
  { value: '3', label: 'Years growing in DevOps and cloud operations' },
  { value: '6+', label: 'Years working with SQL-backed systems' },
]

const highlights = [
  {
    icon: ServerCog,
    title: 'Backend architecture',
    text: 'API design, authentication, async jobs, stored procedures, and production-ready workflows.',
  },
  {
    icon: Cloud,
    title: 'Cloud and DevOps',
    text: 'AWS, Azure, Docker, CI/CD, Nginx, SSL/TLS, monitoring, and release operations.',
  },
  {
    icon: ShieldCheck,
    title: 'Secure fintech logic',
    text: 'Ledger operations, permissions, onboarding, reconciliation, and database-level reliability.',
  },
]

const projects: Project[] = [
  {
    title: 'Mindanao Live',
    eyebrow: 'Full-Stack / DevOps',
    summary:
      'A website I built independently from scratch and deployed to Azure with production infrastructure setup.',
    image: 'https://mindanao.blob.core.windows.net/images/optimized/about-us/original.webp',
    link: 'https://mindanao.live/',
    impact: [
      'Created the entire website alone from scratch.',
      'Deployed and configured the site on Azure.',
      'Set up SSL and CDN for secure delivery and better performance.',
    ],
    stack: ['Azure', 'SSL', 'CDN'],
  },
  {
    title: 'AIMHI Construction SaaS Platform',
    eyebrow: 'Backend Engineer / DevOps Engineer',
    summary:
      'A construction analytics platform for productivity tracking, cost visibility, reconciliation, and operational dashboards.',
    image: '/portfolio/projects/aimhi-dashboard.png',
    impact: [
      'Built authentication, analytics, and dashboard APIs using Flask and PostgreSQL.',
      'Optimized database queries for faster reporting and smoother dashboard usage.',
      'Managed AWS infrastructure, Docker deployments, Bitbucket Pipelines, Nginx, and SSL renewals.',
    ],
    stack: ['Flask', 'PostgreSQL', 'AWS', 'Docker', 'Celery'],
  },
  {
    title: 'SolomonAI Education SaaS',
    eyebrow: 'Backend Engineer / DevOps Engineer',
    summary:
      'An education SaaS that turns uploaded files, web pages, and video transcripts into quizzes, assignments, feedback, and scoring workflows.',
    image: '/portfolio/projects/ai-saas.png',
    impact: [
      'Designed Django REST APIs for quiz generation, preview, finalization, submission, and scoring.',
      'Built Azure OpenAI pipelines with structured JSON parsing and validation.',
      'Integrated Stripe subscriptions, usage tracking, Azure Blob Storage, and production deployments.',
    ],
    stack: ['Django REST', 'PostgreSQL', 'Azure OpenAI', 'Stripe', 'React'],
  },
  {
    title: 'TraxionTech FinTech Backend System',
    eyebrow: 'Backend Software Engineer',
    summary:
      'Secure transaction and onboarding systems for a fintech environment with strict database, permission, and financial logic requirements.',
    image: '/portfolio/projects/fintech.png',
    impact: [
      'Created onboarding and registration APIs with Django REST Framework and Swagger documentation.',
      'Worked on authentication, authorization, permissions, transactions, and ledger-related stored procedures.',
      'Collaborated closely with technical leadership on financial backend logic.',
    ],
    stack: ['Django REST', 'MySQL', 'MSSQL', 'Firebase', 'Swagger'],
  },
]

const experience = [
  {
    role: 'Backend Software Engineer / DevOps Engineer',
    company: 'AIMHI By Eve',
    period: 'Sep 2024 - Feb 2026',
    text: 'Owned backend APIs, Celery workflows, AWS infrastructure, deployments, production monitoring, and mentorship for intern developers.',
  },
  {
    role: 'Full-Stack Software Engineer / DevOps',
    company: 'Freelance and Upwork',
    period: 'Oct 2023 - Aug 2024',
    text: 'Delivered Django, Vue, React, AWS, Azure, Stripe, veterinary, workforce, AI SaaS, and online notepad projects across multiple clients.',
  },
  {
    role: 'Backend Software Engineer',
    company: 'TraxionTech, Inc.',
    period: 'Jun 2021 - Sep 2023',
    text: 'Built fintech APIs, optimized backend performance, and worked across Django REST Framework, MySQL, MSSQL, and Firebase.',
  },
  {
    role: 'Junior Full-Stack Software Developer',
    company: 'Freelance University Projects',
    period: 'Mar 2020 - Apr 2021',
    text: 'Built early production web applications with PHP, Laravel, JavaScript, jQuery, MySQL, Apache, Composer, and XAMPP.',
  },
]

const skills = [
  'Python',
  'Django',
  'Flask',
  'Django REST Framework',
  'PostgreSQL',
  'MSSQL',
  'MySQL',
  'Vue.js 3',
  'Pinia',
  'JavaScript',
  'Docker',
  'Celery',
  'AWS EC2',
  'AWS RDS',
  'AWS S3',
  'Azure VM',
  'Azure Storage',
  'Azure AI',
  'Stripe',
  'CI/CD',
  'Nginx',
  'Linux',
]

const closeMenu = (sectionId?: string) => {
  if (sectionId) {
    activeSection.value = sectionId
  }
  isMenuOpen.value = false
}

const markImageMissing = (path: string) => {
  missingImages.value = new Set(missingImages.value).add(path)
}

const isImageMissing = (path: string) => missingImages.value.has(path)

let navSections: HTMLElement[] = []
let scrollFrame = 0

const updateActiveSection = () => {
  const headerOffset = 110
  const currentPosition = window.scrollY + headerOffset
  const lastSection = navSections
    .filter((section) => section.offsetTop <= currentPosition)
    .at(-1)

  activeSection.value = lastSection?.id ?? 'home'
}

const queueActiveSectionUpdate = () => {
  if (scrollFrame) {
    return
  }

  scrollFrame = window.requestAnimationFrame(() => {
    updateActiveSection()
    scrollFrame = 0
  })
}

onMounted(() => {
  navSections = navItems
    .map((item) => document.querySelector<HTMLElement>(item.href))
    .filter((section): section is HTMLElement => Boolean(section))

  updateActiveSection()
  window.addEventListener('scroll', queueActiveSectionUpdate, { passive: true })
  window.addEventListener('resize', queueActiveSectionUpdate)
})

onUnmounted(() => {
  window.removeEventListener('scroll', queueActiveSectionUpdate)
  window.removeEventListener('resize', queueActiveSectionUpdate)

  if (scrollFrame) {
    window.cancelAnimationFrame(scrollFrame)
  }
})
</script>

<template>
  <div class="site-shell">
    <header class="site-header">
      <a class="brand" href="#home" aria-label="Joshua Bariñan portfolio home" @click="closeMenu('home')">
        <span class="brand-mark">
          <img v-if="!isImageMissing(logoImage)" :src="logoImage" alt="" @error="markImageMissing(logoImage)" />
          <span v-else>JB</span>
        </span>
        <span>
          <strong>Joshua.dev</strong>
          <small>Backend Engineer</small>
        </span>
      </a>

      <button
        class="menu-button"
        type="button"
        :aria-expanded="isMenuOpen"
        aria-label="Toggle navigation"
        @click="isMenuOpen = !isMenuOpen"
      >
        <X v-if="isMenuOpen" :size="20" />
        <Menu v-else :size="20" />
      </button>

      <nav class="site-nav" :class="{ 'site-nav--open': isMenuOpen }" aria-label="Primary navigation">
        <a
          v-for="item in navItems"
          :key="item.href"
          :href="item.href"
          :class="{ 'is-active': activeSection === item.href.slice(1) }"
          @click="closeMenu(item.href.slice(1))"
        >
          {{ item.label }}
        </a>
      </nav>
    </header>

    <main>
      <section id="home" class="hero-section section-pad">
        <div class="hero-copy">
          <p class="eyebrow">
            <BadgeCheck :size="16" />
            Backend-focused Full-Stack Software Engineer
          </p>
          <h1>Reliable APIs, fintech systems, and cloud deployments built with production discipline.</h1>
          <p class="hero-lede">
            I am Joshua Bariñan, a Davao-based engineer with 7+ years of experience building scalable
            web applications, secure backend workflows, database-heavy systems, and AWS/Azure infrastructure.
          </p>

          <div class="hero-actions">
            <a class="button button-primary" href="#projects">
              View work
              <ArrowUpRight :size="18" />
            </a>
            <a class="button button-secondary" href="mailto:barinanjoshua@gmail.com">
              <Mail :size="18" />
              Email me
            </a>
          </div>

          <dl class="hero-stats" aria-label="Career highlights">
            <div v-for="stat in stats" :key="stat.label">
              <dt>{{ stat.value }}</dt>
              <dd>{{ stat.label }}</dd>
            </div>
          </dl>
        </div>

        <aside class="profile-panel" aria-label="Profile summary">
          <div class="profile-image-wrap">
            <img
              v-if="!isImageMissing(profileImage)"
              :src="profileImage"
              alt="Joshua Bariñan"
              @error="markImageMissing(profileImage)"
            />
            <div v-else class="profile-fallback" aria-hidden="true">
              <span>JB</span>
            </div>
          </div>
          <div class="profile-card">
            <p class="profile-name">Joshua Bariñan</p>
            <p>Python, Django/Flask, SQL, AWS, Azure, CI/CD</p>
            <div class="profile-location">
              <MapPin :size="16" />
              Davao City, Philippines
            </div>
          </div>
        </aside>
      </section>

      <section class="section-pad compact-band" aria-label="Core strengths">
        <article v-for="item in highlights" :key="item.title" class="highlight-card">
          <component :is="item.icon" :size="22" />
          <h2>{{ item.title }}</h2>
          <p>{{ item.text }}</p>
        </article>
      </section>

      <section id="about" class="section-pad about-section">
        <div class="section-heading">
          <p class="eyebrow">
            <Code2 :size="16" />
            About
          </p>
          <h2>Backend engineer growing deeper into DevOps, cloud operations, and technical leadership.</h2>
        </div>

        <div class="about-grid">
          <div class="about-story">
            <p class="about-kicker">I build from the backend outward.</p>
            <p>
              I work best at the point where backend architecture, database design, and deployment reality meet.
              My background spans fintech transaction systems, construction analytics, AI SaaS products, and
              full-stack client work.
            </p>
            <p>
              I care about clean APIs, predictable release flows, good database boundaries, and readable systems
              that future engineers can safely extend. Recent work includes Celery background services, Stripe
              billing flows, Azure OpenAI integrations, and AWS production operations.
            </p>
          </div>

          <div class="about-details">
            <div>
              <span>Education</span>
              <strong>BS Information Technology</strong>
              <p>University of Southeastern Philippines</p>
            </div>
            <div>
              <span>Focus</span>
              <strong>Backend, DevOps, FinTech</strong>
              <p>APIs, SQL, cloud infrastructure, CI/CD</p>
            </div>
            <div>
              <span>Working style</span>
              <strong>Ownership with calm execution</strong>
              <p>From implementation and debugging to deployment and monitoring.</p>
            </div>
          </div>
        </div>
      </section>

      <section id="projects" class="section-pad projects-section">
        <div class="section-heading section-heading--row">
          <div>
            <p class="eyebrow">
              <BriefcaseBusiness :size="16" />
              Selected projects
            </p>
            <h2>Client-facing work with strong backend foundations.</h2>
          </div>
          <a class="text-link" href="https://github.com/joshbarinan" target="_blank" rel="noreferrer">
            GitHub
            <ArrowUpRight :size="16" />
          </a>
        </div>

        <div class="project-list">
          <article v-for="project in projects" :key="project.title" class="project-card">
            <component
              :is="project.link ? 'a' : 'div'"
              class="project-media"
              :class="{ 'project-media--link': project.link }"
              :href="project.link"
              target="_blank"
              rel="noreferrer"
            >
              <img
                v-if="project.image && !isImageMissing(project.image)"
                :src="project.image"
                :alt="project.title"
                @error="markImageMissing(project.image)"
              />
              <div v-else class="project-fallback" aria-hidden="true">
                <ServerCog :size="36" />
                <span>{{ project.eyebrow }}</span>
              </div>
            </component>
            <div class="project-body">
              <p class="project-eyebrow">{{ project.eyebrow }}</p>
              <h3>
                <a v-if="project.link" class="project-title-link" :href="project.link" target="_blank" rel="noreferrer">
                  {{ project.title }}
                  <ArrowUpRight :size="18" />
                </a>
                <template v-else>{{ project.title }}</template>
              </h3>
              <p>{{ project.summary }}</p>

              <a
                v-if="project.link"
                class="button button-primary project-link"
                :href="project.link"
                target="_blank"
                rel="noreferrer"
              >
                Visit site
                <ArrowUpRight :size="16" />
              </a>

              <ul>
                <li v-for="item in project.impact" :key="item">
                  <CheckCircle2 :size="16" />
                  <span>{{ item }}</span>
                </li>
              </ul>

              <div class="tag-row" aria-label="Technology stack">
                <span v-for="tag in project.stack" :key="tag">{{ tag }}</span>
              </div>
            </div>
          </article>
        </div>
      </section>

      <section id="experience" class="section-pad experience-section">
        <div class="section-heading">
          <p class="eyebrow">
            <CalendarDays :size="16" />
            Experience
          </p>
          <h2>A steady path through backend systems, product work, and production operations.</h2>
        </div>

        <div class="timeline">
          <article v-for="item in experience" :key="`${item.company}-${item.period}`" class="timeline-item">
            <div class="timeline-date">{{ item.period }}</div>
            <div>
              <h3>{{ item.role }}</h3>
              <p class="company">{{ item.company }}</p>
              <p>{{ item.text }}</p>
            </div>
          </article>
        </div>
      </section>

      <section class="section-pad skills-section">
        <div class="section-heading">
          <div>
            <p class="eyebrow">
              <Database :size="16" />
              Technical stack
            </p>
            <h2>Tools I use to build, ship, and maintain production software.</h2>
          </div>
        </div>

        <div class="skill-cloud" aria-label="Skills">
          <span v-for="skill in skills" :key="skill">{{ skill }}</span>
        </div>
      </section>

      <section id="contact" class="section-pad contact-section">
        <div class="contact-card">
          <p class="eyebrow">
            <Sparkles :size="16" />
            Contact
          </p>
          <h2>Have a backend, cloud, or product build that needs a careful engineer?</h2>
          <p>
            I am open to backend, full-stack, DevOps, and fintech-focused work where reliability,
            clean implementation, and ownership matter.
          </p>

          <div class="contact-actions">
            <a class="button button-primary" href="mailto:barinanjoshua@gmail.com">
              <Mail :size="18" />
              Email
            </a>
            <a class="button button-secondary" href="tel:+639463356877">
              <Phone :size="18" />
              Call
            </a>
            <a
              class="button button-secondary"
              href="https://www.linkedin.com/in/joshua-barinan"
              target="_blank"
              rel="noreferrer"
            >
              <Linkedin :size="18" />
              LinkedIn
            </a>
            <a class="button button-secondary" href="https://github.com/joshbarinan" target="_blank" rel="noreferrer">
              <Github :size="18" />
              GitHub
            </a>
          </div>
        </div>
      </section>
    </main>

    <footer class="site-footer">
      <span>© {{ currentYear }} Joshua Bariñan</span>
      <span>Backend Engineer · Davao City, Philippines</span>
    </footer>
  </div>
</template>
