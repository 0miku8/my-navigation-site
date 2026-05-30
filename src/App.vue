<template>
  <div :class="['page', { dark: isDark }]">
    <aside class="sidebar">
      <div class="sidebar-inner">
        <div class="theme-btn-wrap">
          <button class="theme-btn" @click="toggleTheme" :aria-label="isDark ? '切换浅色模式' : '切换深色模式'">
            {{ isDark ? '☀️' : '🌙' }}
          </button>
        </div>
        <a class="nav-item active" href="#">Nexus</a>
      </div>
      <div class="resize-handle" @mousedown="startResize"></div>
    </aside>
    <div class="page-content">
      <header class="site-header">
        <div class="brand">Nexus</div>
      </header>
      <div class="divider"></div>
      <div class="search-bar">
        <input class="search-input" type="text" placeholder="你所想" />
      </div>
      <main class="content">
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const isDark = ref(true);

function toggleTheme() {
  isDark.value = !isDark.value;
}

function startResize(e) {
  e.preventDefault();
  const startX = e.clientX;
  const startWidth = document.querySelector('.sidebar').offsetWidth;

  function onMouseMove(e) {
    const newWidth = Math.max(160, Math.min(500, startWidth + e.clientX - startX));
    document.querySelector('.sidebar').style.width = newWidth + 'px';
  }

  function onMouseUp() {
    document.removeEventListener('mousemove', onMouseMove);
    document.removeEventListener('mouseup', onMouseUp);
    document.body.style.cursor = '';
    document.body.style.userSelect = '';
  }

  document.addEventListener('mousemove', onMouseMove);
  document.addEventListener('mouseup', onMouseUp);
  document.body.style.cursor = 'col-resize';
  document.body.style.userSelect = 'none';
}
</script>

<style scoped>
.page {
  width: 100%;
  max-width: none;
  margin: 0;
  min-height: 100vh;
  background: #1e1e1e;
  color: #e0e0e0;
  box-sizing: border-box;
  display: flex;
}

.page:not(.dark) {
  background: #ededed;
  color: #000000;
}

.site-header {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px 0 0;
  position: relative;
}

.brand {
  font-size: 5em;
  font-weight: 700;
  letter-spacing: 0.24em;
}

.divider {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  margin: 50px 0;
}

.page.dark .divider {
  border-bottom: 1px solid rgba(180, 180, 180, 0.18);
}

  .theme-btn-wrap {
    display: flex;
    justify-content: center;
  }

  .theme-btn {
    width: 36px;
    height: 36px;
    border: none;
    border-radius: 50%;
    background: transparent;
    color: #e0e0e0;
    cursor: pointer;
    font-size: 1.2rem;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
  }

  .page:not(.dark) .theme-btn {
    color: #000000;
  }

  .site-header,
  .hero,
  .card {
    transition: background-color 0.3s ease, color 0.3s ease;
  }

  .page.dark .hero-tag,
  .page.dark .card {
    background: #111827;
    border-color: #1f2937;
  }

  .page.dark .hero-tag {
    color: #cbd5e1;
  }

  .page.dark .hero p,
  .page.dark .cards-label,
  .page.dark .card p,
  .page.dark .card h3 {
    color: #cbd5e1;
  }

  .page.dark .hero-actions .btn.primary {
    background: #1d4ed8;
    color: #ffffff;
  }

  .page.dark .hero-actions .btn.secondary {
    background: #1f2937;
    color: #e0e0e0;
  }

  .page.dark .hero-visual .visual-ring {
    background: radial-gradient(circle at 30% 30%, rgba(59, 130, 246, 0.22), transparent 24%),
      radial-gradient(circle at 70% 20%, rgba(99, 102, 241, 0.18), transparent 22%),
      radial-gradient(circle at 50% 70%, rgba(59, 130, 246, 0.12), transparent 28%),
      #0f172a;
    box-shadow: 0 40px 120px rgba(59, 130, 246, 0.18);
  }

  .page.dark .visual-play {
    background: #0f172a;
    box-shadow: 0 30px 80px rgba(50, 50, 50, 0.4);
  }

.hero {
  display: grid;
  grid-template-columns: 1.3fr 0.7fr;
  gap: 64px;
  align-items: center;
  padding-bottom: 40px;
}

.hero-copy {
}

.hero-tag {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  border-radius: 999px;
  background: #111827;
  border: 1px solid #1f2937;
  color: #cbd5e1;
  font-size: 0.95rem;
  margin-bottom: 24px;
}

.hero h1 {
  margin: 0;
  font-size: clamp(3rem, 6vw, 5rem);
  line-height: 0.95;
  font-weight: 800;
  letter-spacing: -0.05em;
  background: linear-gradient(90deg, #1e40af, #2563eb, #60a5fa);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}

.hero p {
  margin-top: 24px;
  font-size: 1.1rem;
  line-height: 1.7;
    color: #cbd5e1;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 32px;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 14px 22px;
  border-radius: 999px;
  font-weight: 700;
  text-decoration: none;
  transition: transform 0.2s ease, background 0.2s ease;
}

.btn:hover {
  transform: translateY(-1px);
}

.primary {
  background: #2563eb;
  color: white;
}

.secondary {
  background: #334155;
  color: #f8fafc;
}

.hero-visual {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 520px;
}

.visual-ring {
  width: min(520px, 100%);
  aspect-ratio: 1;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, rgba(59, 130, 246, 0.35), transparent 24%),
    radial-gradient(circle at 70% 20%, rgba(99, 102, 241, 0.3), transparent 22%),
    radial-gradient(circle at 50% 70%, rgba(59, 130, 246, 0.18), transparent 28%),
    #eff6ff;
  box-shadow: 0 40px 120px rgba(59, 130, 246, 0.15);
}

.visual-play {
  position: absolute;
  width: 180px;
  height: 180px;
  clip-path: polygon(0 0, 100% 50%, 0 100%);
  background: white;
  box-shadow: 0 30px 80px rgba(50, 50, 50, 0.18);
}

.cards-section {
  padding: 24px 0 40px;
}

.cards-label {
  text-align: center;
  font-size: 1rem;
  color: #475569;
  margin-bottom: 24px;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(240px, 1fr));
  gap: 24px;
}

.card {
  background: #111827;
  border: 1px solid #1f2937;
  border-radius: 28px;
  padding: 24px;
  min-height: 190px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 10px 30px rgba(50, 50, 50, 0.05);
}

.card-icon {
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  border-radius: 14px;
    background: #0f172a;
    box-shadow: inset 0 1px 2px rgba(50, 50, 50, 0.06);
    font-size: 1.25rem;
  }

  .card h3 {
    margin: 0;
    font-size: 1.05rem;
    color: #e0e0e0;
  }

  .card p {
    margin: 0;
    color: #cbd5e1;
  .hero-visual {
    min-height: 320px;
  }
}

.sidebar {
  width: var(--sidebar-width, 240px);
  flex-shrink: 0;
  background: #2c2c2c;
  border-right: 1px solid rgba(180, 180, 180, 0.12);
  box-sizing: border-box;
  position: relative;
}

.resize-handle {
  position: absolute;
  top: 0;
  right: 0;
  width: 6px;
  height: 100%;
  cursor: col-resize;
  z-index: 10;
  transition: background 0.15s;
}

.resize-handle:hover,
.resize-handle:active {
  background: rgba(180, 180, 180, 0.3);
}

.page:not(.dark) .sidebar {
  background: #ffffff;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
}

.sidebar-inner {
  padding: 24px 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  position: sticky;
  top: 0;
}

.page-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 32px 48px;
  min-width: 0;
}

.nav-item {
  display: block;
  padding: 10px 14px;
  border-radius: 8px;
  font-size: 0.95rem;
  color: inherit;
  text-decoration: none;
  opacity: 0.75;
  transition: background 0.2s, opacity 0.2s;
}

.nav-item:hover {
  background: rgba(180, 180, 180, 0.1);
  opacity: 1;
}

.nav-item.active {
  opacity: 1;
  font-weight: 600;
  background: rgba(180, 180, 180, 0.08);
}

.search-bar {
  margin-bottom: 24px;
}

.search-input {
  width: 100%;
  max-width: 400px;
  padding: 10px 16px;
  border-radius: 999px;
  border: 1px solid rgba(180, 180, 180, 0.3);
  background: rgba(50, 50, 50, 0.5);
  color: #e0e0e0;
  font-size: 0.95rem;
  box-sizing: border-box;
}

.search-input::placeholder {
  color: #888888;
}

.page:not(.dark) .search-input {
  background: #ffffff;
  color: #000000;
  border-color: rgba(0, 0, 0, 0.15);
}

.content {
  flex: 1;
  min-width: 0;
}

@media (max-width: 640px) {
  .page {
    padding: 16px;
  }

  .site-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .site-nav {
    justify-content: flex-start;
  }
}
</style>