<template>
  <div :class="['page', { dark: isDark }]" @keydown.escape="previewImg = ''">
    <aside class="sidebar">
      <div class="sidebar-inner">
        <div class="theme-btn-wrap">
          <button class="theme-btn" @click="toggleTheme" :aria-label="isDark ? '切换浅色模式' : '切换深色模式'">
            {{ isDark ? '☀️' : '🌙' }}
          </button>
        </div>
        <a class="nav-item" v-for="item in navItems" :key="item.id" :class="{ active: currentPage === item.id }" href="#" @click.prevent="currentPage = item.id">{{ item.label }}</a>
      </div>
      <div class="resize-handle" @mousedown="startResize"></div>
    </aside>
    <div class="page-content">
      <template v-if="currentPage === 'home'">
        <header class="site-header">
          <div class="brand">Nexus</div>
        </header>
        <div class="divider"></div>
        <div class="search-bar">
          <div class="search-wrapper">
            <input class="search-input" type="text" placeholder="搜索网页或软件" v-model="searchQuery" @keydown.enter="doSearch" />
            <button class="search-btn" @click="doSearch">🔍</button>
          </div>
          <div v-if="searchQuery && allSearchItems.length" class="search-results">
            <div v-for="item in allSearchItems" :key="item.label" class="search-result-item" @click="goTo(item)">
              {{ item.label }}
            </div>
          </div>
        </div>
        </template>
        <div class="divider"></div>
      <main class="content">
        <div class="page-transition">
        <template v-if="currentPage === 'home'">
          <div class="all-cards">
            <a class="tool-card" v-for="item in allToolItems" :key="item.label" :href="item.url" target="_blank" rel="noopener">
              <span class="tool-icon">{{ item.icon }}</span>
              <div class="tool-info">
                <span class="tool-name">{{ item.label }}</span>
                <span class="tool-desc">{{ item.desc }}</span>
              </div>
            </a>
          </div>
        </template>
        <div v-if="currentPage === 'web'" class="web-page">
          <div class="category-section">
            <h2 class="category-title">编程学习</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://www.runoob.com" target="_blank" rel="noopener">
                <span class="tool-icon">📘</span>
                <div class="tool-info">
                  <span class="tool-name">菜鸟教程</span>
                  <span class="tool-desc">编程基础在线教程</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.codedex.io" target="_blank" rel="noopener">
                <span class="tool-icon">💻</span>
                <div class="tool-info">
                  <span class="tool-name">Codédex</span>
                  <span class="tool-desc">交互式编程学习平台</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com" target="_blank" rel="noopener">
                <span class="tool-icon">💻</span>
                <div class="tool-info">
                  <span class="tool-name">GitHub</span>
                  <span class="tool-desc">开源代码托管平台</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">在线工具</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://test.ustc.edu.cn/" target="_blank" rel="noopener">
                <span class="tool-icon">🌐</span>
                <div class="tool-info">
                  <span class="tool-name">USTC 网络测速</span>
                  <span class="tool-desc">校园网速度测试</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">设计灵感</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://coverbox.henry-hu.com/" target="_blank" rel="noopener">
                <span class="tool-icon">🎨</span>
                <div class="tool-info">
                  <span class="tool-name">Coverbox</span>
                  <span class="tool-desc">专辑封面查找</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.pinterest.com" target="_blank" rel="noopener">
                <span class="tool-icon">📌</span>
                <div class="tool-info">
                  <span class="tool-name">Pinterest</span>
                  <span class="tool-desc">视觉灵感与创意收集</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.pixiv.net" target="_blank" rel="noopener">
                <span class="tool-icon">🖼️</span>
                <div class="tool-info">
                  <span class="tool-name">Pixiv</span>
                  <span class="tool-desc">日本插画艺术社区</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">摄影参考</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://www.bodiesinmotion.photo" target="_blank" rel="noopener">
                <span class="tool-icon">🏃</span>
                <div class="tool-info">
                  <span class="tool-name">Bodies in Motion</span>
                  <span class="tool-desc">人体动态参考摄影</span>
                </div>
              </a>
              <a class="tool-card" href="https://vutoka.com/" target="_blank" rel="noopener">
                <span class="tool-icon">📷</span>
                <div class="tool-info">
                  <span class="tool-name">Vu Toka</span>
                  <span class="tool-desc">摄影师人像作品参考</span>
                </div>
              </a>
              <a class="tool-card" href="https://500px.com.cn" target="_blank" rel="noopener">
                <span class="tool-icon">📸</span>
                <div class="tool-info">
                  <span class="tool-name">500px</span>
                  <span class="tool-desc">全球摄影师作品社区</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">游戏模组</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://www.nexusmods.com" target="_blank" rel="noopener">
                <span class="tool-icon">🎮</span>
                <div class="tool-info">
                  <span class="tool-name">Nexus Mods</span>
                  <span class="tool-desc">全球最大游戏模组社区</span>
                </div>
              </a>
              <a class="tool-card" href="https://mdmc.moe/charts" target="_blank" rel="noopener">
                <span class="tool-icon">🎵</span>
                <div class="tool-info">
                  <span class="tool-name">Muse Dash 模组社区</span>
                  <span class="tool-desc">二次元音游与模组</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">影音娱乐</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://mikanani.me" target="_blank" rel="noopener">
                <span class="tool-icon">🍊</span>
                <div class="tool-info">
                  <span class="tool-name">蜜柑计划</span>
                  <span class="tool-desc">动漫资源字幕下载</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.midishow.com" target="_blank" rel="noopener">
                <span class="tool-icon">🎹</span>
                <div class="tool-info">
                  <span class="tool-name">MidiShow</span>
                  <span class="tool-desc">MIDI 音乐资源分享</span>
                </div>
              </a>
              <a class="tool-card" href="https://piastudy.com/" target="_blank" rel="noopener">
                <span class="tool-icon">🎹</span>
                <div class="tool-info">
                  <span class="tool-name">天天钢琴</span>
                  <span class="tool-desc">钢琴谱与免费在线课程</span>
                </div>
              </a>
            </div>
          </div>
        </div>
        <div v-else-if="currentPage === 'software'" class="software-page">
          <div class="category-section">
            <h2 class="category-title">系统工具</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://www.tbtool.cn" target="_blank" rel="noopener">
                <span class="tool-icon">🔧</span>
                <div class="tool-info">
                  <span class="tool-name">图吧工具箱</span>
                  <span class="tool-desc">硬件检测与系统工具合集</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/Diorser/LiteMonitor" target="_blank" rel="noopener">
                <span class="tool-icon">📊</span>
                <div class="tool-info">
                  <span class="tool-name">LiteMonitor</span>
                  <span class="tool-desc">轻量级系统监控工具</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.nvidia.com/en-us/software/nvidia-app/" target="_blank" rel="noopener">
                <span class="tool-icon">🟢</span>
                <div class="tool-info">
                  <span class="tool-name">NVIDIA App</span>
                  <span class="tool-desc">NVIDIA 驱动与游戏优化</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">文件工具</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://localsend.org" target="_blank" rel="noopener">
                <span class="tool-icon">📤</span>
                <div class="tool-info">
                  <span class="tool-name">LocalSend</span>
                  <span class="tool-desc">跨平台局域网文件传输</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/Tichau/FileConverter" target="_blank" rel="noopener">
                <span class="tool-icon">🔄</span>
                <div class="tool-info">
                  <span class="tool-name">FileConverter</span>
                  <span class="tool-desc">文件格式批量转换</span>
                </div>
              </a>
              <a class="tool-card" href="https://potplayer.daum.net" target="_blank" rel="noopener">
                <span class="tool-icon">📺</span>
                <div class="tool-info">
                  <span class="tool-name">PotPlayer</span>
                  <span class="tool-desc">全能视频与音乐播放器</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">图形与设计</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://www.systemax.jp/en/sai/" target="_blank" rel="noopener">
                <span class="tool-icon">🎨</span>
                <div class="tool-info">
                  <span class="tool-name">PaintTool SAI2</span>
                  <span class="tool-desc">轻量级数码绘画软件</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.live2d.com" target="_blank" rel="noopener">
                <span class="tool-icon">🎭</span>
                <div class="tool-info">
                  <span class="tool-name">Live2D Cubism</span>
                  <span class="tool-desc">2D 角色动画制作软件</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.topazlabs.com/topaz-photo-ai" target="_blank" rel="noopener">
                <span class="tool-icon">🤖</span>
                <div class="tool-info">
                  <span class="tool-name">Topaz Photo AI</span>
                  <span class="tool-desc">AI 图像画质增强软件</span>
                </div>
              </a>
              <a class="tool-card" href="https://imageglass.org" target="_blank" rel="noopener">
                <span class="tool-icon">🖼️</span>
                <div class="tool-info">
                  <span class="tool-name">ImageGlass</span>
                  <span class="tool-desc">轻量级图片查看器</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">影音与创作</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://www.blackmagicdesign.com/products/davinciresolve" target="_blank" rel="noopener">
                <span class="tool-icon">🎬</span>
                <div class="tool-info">
                  <span class="tool-name">DaVinci Resolve</span>
                  <span class="tool-desc">专业视频剪辑与调色软件</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.presonus.com/en-us/studio-one/" target="_blank" rel="noopener">
                <span class="tool-icon">🎵</span>
                <div class="tool-info">
                  <span class="tool-name">Studio One 7</span>
                  <span class="tool-desc">专业数字音频工作站</span>
                </div>
              </a>
              <a class="tool-card" href="https://obsproject.com" target="_blank" rel="noopener">
                <span class="tool-icon">📹</span>
                <div class="tool-info">
                  <span class="tool-name">OBS Studio</span>
                  <span class="tool-desc">免费直播与录屏软件</span>
                </div>
              </a>
              <a class="tool-card" href="https://sites.google.com/view/sequator" target="_blank" rel="noopener">
                <span class="tool-icon">✨</span>
                <div class="tool-info">
                  <span class="tool-name">Sequator</span>
                  <span class="tool-desc">天文摄影堆栈合成软件</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">开发与效率</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://opencode.ai" target="_blank" rel="noopener">
                <span class="tool-icon">🤖</span>
                <span class="tool-name">OpenCode</span>
                <span class="tool-desc">AI 编程助手工具</span>
              </a>
              <a class="tool-card" href="https://github.com/Ceceliaee/time-tracking" target="_blank" rel="noopener">
                <span class="tool-icon">⏱️</span>
                <div class="tool-info">
                  <span class="tool-name">Time Tracking</span>
                  <span class="tool-desc">时间追踪与效率管理</span>
                </div>
              </a>
              <a class="tool-card" href="https://code.visualstudio.com" target="_blank" rel="noopener">
                <span class="tool-icon">📝</span>
                <div class="tool-info">
                  <span class="tool-name">VS Code</span>
                  <span class="tool-desc">轻量级代码编辑器</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.jetbrains.com/pycharm/" target="_blank" rel="noopener">
                <span class="tool-icon">🐍</span>
                <div class="tool-info">
                  <span class="tool-name">PyCharm</span>
                  <span class="tool-desc">Python 专业 IDE</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">整理下载</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://eagle.cool" target="_blank" rel="noopener">
                <span class="tool-icon">🦅</span>
                <div class="tool-info">
                  <span class="tool-name">Eagle</span>
                  <span class="tool-desc">设计师素材管理工具</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/venera-app/venera" target="_blank" rel="noopener">
                <span class="tool-icon">📖</span>
                <div class="tool-info">
                  <span class="tool-name">Venera</span>
                  <span class="tool-desc">漫画阅读与下载工具</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/fish2018/pansou" target="_blank" rel="noopener">
                <span class="tool-icon">🔍</span>
                <div class="tool-info">
                  <span class="tool-name">盘搜</span>
                  <span class="tool-desc">网盘资源搜索工具</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/yaobiao131/downkyicore" target="_blank" rel="noopener">
                <span class="tool-icon">📥</span>
                <div class="tool-info">
                  <span class="tool-name">DownKyi</span>
                  <span class="tool-desc">Bilibili 视频下载工具</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/xuejianxianzun/PixivBatchDownloader" target="_blank" rel="noopener">
                <span class="tool-icon">🖼️</span>
                <div class="tool-info">
                  <span class="tool-name">Pixiv Batch Downloader</span>
                  <span class="tool-desc">Pixiv 批量下载工具</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/ciromattia/kcc" target="_blank" rel="noopener">
                <span class="tool-icon">📱</span>
                <div class="tool-info">
                  <span class="tool-name">KCC</span>
                  <span class="tool-desc">Kindle 漫画格式转换</span>
                </div>
              </a>
              <a class="tool-card" href="https://www.internetdownloadmanager.com" target="_blank" rel="noopener">
                <span class="tool-icon">⚡</span>
                <div class="tool-info">
                  <span class="tool-name">Internet Download Manager</span>
                  <span class="tool-desc">高速下载管理器</span>
                </div>
              </a>
            </div>
          </div>
          <div class="category-section">
            <h2 class="category-title">游戏</h2>
            <div class="tool-grid">
              <a class="tool-card" href="https://store.steampowered.com" target="_blank" rel="noopener">
                <span class="tool-icon">🎮</span>
                <div class="tool-info">
                  <span class="tool-name">Steam</span>
                  <span class="tool-desc">全球最大 PC 游戏平台</span>
                </div>
              </a>
              <a class="tool-card" href="https://github.com/Hex-Dragon/PCL2" target="_blank" rel="noopener">
                <span class="tool-icon">⛏️</span>
                <div class="tool-info">
                  <span class="tool-name">Plain Craft Launcher 2</span>
                  <span class="tool-desc">我的世界启动器</span>
                </div>
              </a>
            </div>
          </div>
        </div>
        <div v-else-if="currentPage === 'music'" class="music-page">
          <div class="artist-search-bar">
            <div class="artist-search-wrapper">
              <input class="artist-search-input" type="text" placeholder="搜索歌曲或歌手..." v-model="musicQuery" />
            </div>
          </div>
          <div class="music-grid">
            <div class="music-card" v-for="(m, idx) in filteredMusic" :key="idx" @click="copyMusic(m, $event)">
              <div class="music-info">
                <div class="music-song">{{ m.song }}</div>
                <div class="music-artist">{{ m.artist }}</div>
              </div>
            </div>
          </div>
        </div>
        <div v-else-if="currentPage === 'artist'" class="artist-page">
          <div class="artist-search-bar">
            <div class="artist-search-wrapper">
              <input class="artist-search-input" type="text" placeholder="搜索画师或标签..." v-model="artistQuery" />
            </div>
          </div>
          <div class="artist-grid">
            <div class="artist-card" v-for="a in filteredArtists" :key="a.name">
              <div class="artist-header">
                <div class="artist-avatar">
                  <img :src="a.avatar" :alt="a.name" @error="onImgError($event)" />
              </div>
              <div class="artist-info">
                  <h3 class="artist-name">{{ a.name }}</h3>
                  <div class="artist-links">
                    <a :href="a.pixiv" target="_blank" rel="noopener" class="pixiv-link">pixiv</a>
                  </div>
                  <div class="artist-tags" v-if="a.tags">
                    <span class="tag" v-for="t in a.tags.split(', ')" :key="t">{{ t }}</span>
                  </div>
                </div>
              </div>
              <div class="artist-works" v-if="a.works && a.works.length">
                  <div class="work-thumb" v-for="(w, wi) in a.works" :key="wi" @click="previewImg = w">
                    <img :src="w" :alt="a.name + ' work ' + (wi+1)" loading="lazy" @error="onImgError($event)" />
              </div>
            </div>
          </div>
        </div>      
        </div>
        </div>
      </main>

      <div class="lightbox" v-if="previewImg" @click="previewImg = ''">
        <img :src="previewImg" class="lightbox-img" />
      </div>
    </div>
  </div>      <div class="toast" v-if="toast" :style="{ left: toast.x + 'px', top: toast.y + 'px' }">
        {{ toast.text }}
      </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const isDark = ref(true);
const currentPage = ref('home');
const searchQuery = ref('');
const previewImg = ref('');
const artistQuery = ref('');
const musicQuery = ref('');

const toast = ref(null);

function copyMusic(m, e) {
  const text = `${m.song} - ${m.artist}`;
  navigator.clipboard.writeText(text).catch(() => {});
  const rect = e.currentTarget.getBoundingClientRect();
  toast.value = { text: '已复制！', x: rect.left + rect.width / 2, y: rect.top };
  setTimeout(() => { toast.value = null; }, 1200);
}

const filteredArtists = computed(() => {
  const q = artistQuery.value.trim().toLowerCase();
  if (!q) return artists;
  return artists.filter(a => {
    if (a.name.toLowerCase().includes(q)) return true;
    if (a.tags && a.tags.toLowerCase().includes(q)) return true;
    return false;
  });
});

  const artists = [
    { name: 'Noyu', avatar: '/nexus/images/artists/noyu.jpg', pixiv: 'https://www.pixiv.net/users/26040235', works: ['/nexus/images/artworks/noyu/1.jpg','/nexus/images/artworks/noyu/2.jpg','/nexus/images/artworks/noyu/3.jpg'] , tags: '少女, 厚涂, 奇幻, 氛围, 人外, 哥特, 色彩'},
    { name: '望月けい', avatar: '/nexus/images/artists/mochizuki_kei.jpg', pixiv: 'https://www.pixiv.net/users/1193008', works: ['/nexus/images/artworks/mochizuki_kei/1.jpg','/nexus/images/artworks/mochizuki_kei/2.jpg','/nexus/images/artworks/mochizuki_kei/3.jpg'] , tags: '厚涂, 帅气, 风格化, 原创, 个性, 三丽鸥, 设计'},
    { name: 'neco', avatar: '/nexus/images/artists/neco.jpg', pixiv: 'https://www.pixiv.net/users/777703', works: ['/nexus/images/artworks/neco/1.jpg','/nexus/images/artworks/neco/2.jpg','/nexus/images/artworks/neco/3.jpg'] , tags: '机甲, 科幻, 重装, 少女, 武器, 军武, 设计'},
    { name: '室埴 ポコ', avatar: '/nexus/images/artists/murohani_poco.jpg', pixiv: 'https://www.pixiv.net/users/76266', works: ['/nexus/images/artworks/murohani_poco/1.jpg','/nexus/images/artworks/murohani_poco/2.jpg','/nexus/images/artworks/murohani_poco/3.jpg'] , tags: '机甲, 机械, 兽耳, 狐狸, 金发, 尾巴, 武器'},
    { name: '赤倉＠画集発売中', avatar: '/nexus/images/artists/akakura.jpg', pixiv: 'https://www.pixiv.net/users/882569', works: ['/nexus/images/artworks/akakura/1.jpg','/nexus/images/artworks/akakura/2.jpg','/nexus/images/artworks/akakura/3.jpg'] , tags: '少女, 可爱, 天使, 恶魔, 制服, 绚丽, 色彩'},
    { name: 'Anmi@画集発売中', avatar: '/nexus/images/artists/anmi.jpg', pixiv: 'https://www.pixiv.net/users/212801', works: ['/nexus/images/artworks/anmi/1.jpg','/nexus/images/artworks/anmi/2.jpg','/nexus/images/artworks/anmi/3.jpg'] , tags: '少女, 唯美, 百合, 治愈, 色彩, 花, 柔光'},
    { name: 'アシマ / Ashima', avatar: '/nexus/images/artists/ashima.jpg', pixiv: 'https://www.pixiv.net/users/2642047', works: ['/nexus/images/artworks/ashima/1.jpg','/nexus/images/artworks/ashima/2.jpg','/nexus/images/artworks/ashima/3.jpg'] , tags: '少女, 幻想, 华丽, 花, 制服, 优雅, 可爱'},
    { name: 'モ誰', avatar: '/nexus/images/artists/mosui.jpg', pixiv: 'https://www.pixiv.net/users/1878082', works: ['/nexus/images/artworks/mosui/1.jpg','/nexus/images/artworks/mosui/2.jpg','/nexus/images/artworks/mosui/3.jpg'] , tags: '厚涂, 韩系, 精致, 美少女, 笑容, 角色, 华丽'},
    { name: 'イコモチ', avatar: '/nexus/images/artists/icomochi.jpg', pixiv: 'https://www.pixiv.net/users/801146', works: ['/nexus/images/artworks/icomochi/1.jpg','/nexus/images/artworks/icomochi/2.jpg','/nexus/images/artworks/icomochi/3.jpg'] , tags: '可爱, VTuber, hololive, FUWAMOCO, 同人, 兽耳, 萌'},
    { name: 'あるてら', avatar: '/nexus/images/artists/arutera.jpg', pixiv: 'https://www.pixiv.net/users/483730', works: ['/nexus/images/artworks/arutera/1.jpg','/nexus/images/artworks/arutera/2.jpg','/nexus/images/artworks/arutera/3.jpg'] , tags: '少女, VTuber, 礼装, 长发, 色彩, 优雅, 动态'},
    { name: '鬼针草', avatar: '/nexus/images/artists/guizhencao.jpg', pixiv: 'https://www.pixiv.net/users/6049901', works: ['/nexus/images/artworks/guizhencao/1.jpg','/nexus/images/artworks/guizhencao/2.jpg','/nexus/images/artworks/guizhencao/3.jpg'] , tags: '少女, 白丝, 制服, 黑丝, 短髮, 明日方舟, 绝对领域'},
    { name: 'ヒトこもる', avatar: '/nexus/images/artists/hitokomoru.jpg', pixiv: 'https://www.pixiv.net/users/30837811', works: ['/nexus/images/artworks/hitokomoru/1.jpg','/nexus/images/artworks/hitokomoru/2.jpg','/nexus/images/artworks/hitokomoru/3.jpg'] , tags: '少女, 可爱, 日常, 治愈, 白髮, 兽耳, 笑容'},
    { name: 'wlop', avatar: '/nexus/images/artists/wlop.jpg', pixiv: 'https://www.pixiv.net/users/2188232', works: ['/nexus/images/artworks/wlop/1.jpg','/nexus/images/artworks/wlop/2.jpg','/nexus/images/artworks/wlop/3.jpg'] , tags: '厚涂, 奇幻, 氛围, 光影, 史诗, 场景, 原创'},
    { name: 'ASK', avatar: '/nexus/images/artists/ask.jpg', pixiv: 'https://www.pixiv.net/users/1980643', works: ['/nexus/images/artworks/ask/1.jpg','/nexus/images/artworks/ask/2.jpg','/nexus/images/artworks/ask/3.jpg'] , tags: '少女, 唯美, FGO, 和风, 礼装, 优雅, 氛围'},
    { name: 'rurudo', avatar: '/nexus/images/artists/rurudo.jpg', pixiv: 'https://www.pixiv.net/users/25760573', works: ['/nexus/images/artworks/rurudo/1.jpg','/nexus/images/artworks/rurudo/2.jpg','/nexus/images/artworks/rurudo/3.jpg'] , tags: '萝莉, 可爱, 少女, 性感, 兔女郎, 萌, 制服'},
    { name: 'しらたま❄', avatar: '/nexus/images/artists/shiratama.jpg', pixiv: 'https://www.pixiv.net/users/705370', works: ['/nexus/images/artworks/shiratama/1.jpg','/nexus/images/artworks/shiratama/2.jpg','/nexus/images/artworks/shiratama/3.jpg'] , tags: '萝莉, 可爱, 少女, 治愈, 星空, 天使, 梦幻'},
    { name: 'こうこうや', avatar: '/nexus/images/artists/kokoya.jpg', pixiv: 'https://www.pixiv.net/users/118616623', works: ['/nexus/images/artworks/kokoya/1.jpg','/nexus/images/artworks/kokoya/2.jpg','/nexus/images/artworks/kokoya/3.jpg'] , tags: '少女, 东方, 背景, 风景, 女仆, 幻想, 可爱'},
    { name: '落舟Pile', avatar: '/nexus/images/artists/luozhou_pile.jpg', pixiv: 'https://www.pixiv.net/users/21304996', works: ['/nexus/images/artworks/luozhou_pile/1.jpg','/nexus/images/artworks/luozhou_pile/2.jpg','/nexus/images/artworks/luozhou_pile/3.jpg'] , tags: '明日方舟, 少女, 插画, 药屋, 厚涂, 色彩, 角色'},
    { name: 'mojo', avatar: '/nexus/images/artists/mojo.jpg', pixiv: 'https://www.pixiv.net/users/94576902', works: ['/nexus/images/artworks/mojo/1.jpg','/nexus/images/artworks/mojo/2.jpg','/nexus/images/artworks/mojo/3.jpg'] , tags: '蔚蓝档案, 机甲, 科幻, 校园, 偶像, 机械, 少女'},
    { name: 'Nbrush19', avatar: '/nexus/images/artists/nbrush19.jpg', pixiv: 'https://www.pixiv.net/users/44895546', works: ['/nexus/images/artworks/nbrush19/1.jpg','/nexus/images/artworks/nbrush19/2.jpg','/nexus/images/artworks/nbrush19/3.jpg'] , tags: '明日方舟, 少女, 插画, Fate, 厚涂, 色彩, 优美'},
    { name: '富士やま', avatar: '/nexus/images/artists/fujiyama.jpg', pixiv: 'https://www.pixiv.net/users/9343974', works: ['/nexus/images/artworks/fujiyama/1.jpg','/nexus/images/artworks/fujiyama/2.jpg','/nexus/images/artworks/fujiyama/3.jpg'] , tags: '少女, 游戏, 可爱, 猫耳, 女仆, 厚涂, 色彩'},
    { name: 'ふぇありぃあい', avatar: '/nexus/images/artists/fairy_eye.jpg', pixiv: 'https://www.pixiv.net/users/1055457', works: ['/nexus/images/artworks/fairy_eye/1.jpg','/nexus/images/artworks/fairy_eye/2.jpg','/nexus/images/artworks/fairy_eye/3.jpg'] , tags: 'hololive, VTuber, 可爱, 少女, 女仆, 同人, 治愈'},
    { name: '宮坂みゆ', avatar: '/nexus/images/artists/miyasaka_miyu.jpg', pixiv: 'https://www.pixiv.net/users/839617', works: ['/nexus/images/artworks/miyasaka_miyu/1.jpg','/nexus/images/artworks/miyasaka_miyu/2.jpg','/nexus/images/artworks/miyasaka_miyu/3.jpg'] , tags: '少女, 可爱, 双马尾, 同人, 粉发, 萝莉, 童话'},
    { name: '飴玉コン', avatar: '/nexus/images/artists/amedama_kon.jpg', pixiv: 'https://www.pixiv.net/users/1992163', works: ['/nexus/images/artworks/amedama_kon/1.jpg','/nexus/images/artworks/amedama_kon/2.jpg','/nexus/images/artworks/amedama_kon/3.jpg'] , tags: '少女, 百合, 厚涂, 萝莉, 白髮, 泳装, 双马尾'},
    { name: 'きょくちょ', avatar: '/nexus/images/artists/kyockcho.jpg', pixiv: 'https://www.pixiv.net/users/22782', works: ['/nexus/images/artworks/kyockcho/1.jpg','/nexus/images/artworks/kyockcho/2.jpg','/nexus/images/artworks/kyockcho/3.jpg'] , tags: '少女, 可爱, 女仆, 漫画, 制服, 校园, 萝莉'},
    { name: '秋タケ', avatar: '/nexus/images/artists/akitake.jpg', pixiv: 'https://www.pixiv.net/users/21076169', works: ['/nexus/images/artworks/akitake/1.jpg','/nexus/images/artworks/akitake/2.jpg','/nexus/images/artworks/akitake/3.jpg'] , tags: '蔚蓝档案, 少女, 可爱, 笑容, 校园, 制服, 阳光'},
    { name: 'ぱるふぇいと', avatar: '/nexus/images/artists/parfait.jpg', pixiv: 'https://www.pixiv.net/users/1179457', works: ['/nexus/images/artworks/parfait/1.jpg','/nexus/images/artworks/parfait/2.jpg','/nexus/images/artworks/parfait/3.jpg'] , tags: '白发, 少女, 萝莉, 可爱, 幻想, 梦幻, 天使'},
    { name: '薄藍', avatar: '/nexus/images/artists/usui.jpg', pixiv: 'https://www.pixiv.net/users/38276220', works: ['/nexus/images/artworks/usui/1.jpg','/nexus/images/artworks/usui/2.jpg','/nexus/images/artworks/usui/3.jpg'] , tags: '少女, FGO, 幻想, 可爱, 天使, 花, 色彩'},
    { name: '種乃なかみ', avatar: '/nexus/images/artists/taneno_nakami.jpg', pixiv: 'https://www.pixiv.net/users/12823252', works: ['/nexus/images/artworks/taneno_nakami/1.jpg','/nexus/images/artworks/taneno_nakami/2.jpg','/nexus/images/artworks/taneno_nakami/3.jpg'] , tags: '少女, 百合, 同人, 可爱, 制服, 马尾, 日常'},
    { name: 'こんぺ伊藤', avatar: '/nexus/images/artists/konpe_ito.jpg', pixiv: 'https://www.pixiv.net/users/17777967', works: ['/nexus/images/artworks/konpe_ito/1.jpg','/nexus/images/artworks/konpe_ito/2.jpg','/nexus/images/artworks/konpe_ito/3.jpg'] , tags: '少女, 女仆, 猫耳, 双马尾, 白髮, 护士, 可爱'},
    { name: 'いずみななせ', avatar: '/nexus/images/artists/izumi_nanase.jpg', pixiv: 'https://www.pixiv.net/users/7145395', works: ['/nexus/images/artworks/izumi_nanase/1.jpg','/nexus/images/artworks/izumi_nanase/2.jpg','/nexus/images/artworks/izumi_nanase/3.jpg'] , tags: 'VTuber, 少女, 委托, VRChat, 猫耳, 可爱, 立绘'},
    { name: 'にんげんまめ', avatar: '/nexus/images/artists/ningen_mame.jpg', pixiv: 'https://www.pixiv.net/users/38297201', works: ['/nexus/images/artworks/ningen_mame/1.jpg','/nexus/images/artworks/ningen_mame/2.jpg','/nexus/images/artworks/ningen_mame/3.jpg'] , tags: '蔚蓝档案, 少女, 校园, 偶像, 制服, 可爱, 青春'},
    { name: 'Dudrinm', avatar: '/nexus/images/artists/dudrinm.jpg', pixiv: 'https://www.pixiv.net/users/94061649', works: ['/nexus/images/artworks/dudrinm/1.jpg','/nexus/images/artworks/dudrinm/2.jpg','/nexus/images/artworks/dudrinm/3.jpg'] , tags: '少女, EVA, 同人, 奇幻, 鸣潮, 色彩, 原创'},
    { name: 'Noah', avatar: '/nexus/images/artists/noah.jpg', pixiv: 'https://www.pixiv.net/users/87145717', works: ['/nexus/images/artworks/noah/1.jpg','/nexus/images/artworks/noah/2.jpg','/nexus/images/artworks/noah/3.jpg'] , tags: 'VTuber, 少女, 可爱, 天使, 幻想, 色彩, 治愈'},
    { name: '梅原生（せい）', avatar: '/nexus/images/artists/umehara_sei.jpg', pixiv: 'https://www.pixiv.net/users/3906246', works: ['/nexus/images/artworks/umehara_sei/1.jpg','/nexus/images/artworks/umehara_sei/2.jpg','/nexus/images/artworks/umehara_sei/3.jpg'] , tags: '古风, 少女, 中华, 原创, 兽耳, 优雅, 清新'},
    { name: 'Sua', avatar: '/nexus/images/artists/sua.jpg', pixiv: 'https://www.pixiv.net/users/9999743', works: ['/nexus/images/artworks/sua/1.jpg','/nexus/images/artworks/sua/2.jpg','/nexus/images/artworks/sua/3.jpg'] , tags: '少女, 鸣潮, 明日方舟, 原创, 色彩, 华丽, 角色'},
    { name: '夢ノ内', avatar: '/nexus/images/artists/yume_no_uchi.jpg', pixiv: 'https://www.pixiv.net/users/229788', works: ['/nexus/images/artworks/yume_no_uchi/1.jpg','/nexus/images/artworks/yume_no_uchi/2.jpg','/nexus/images/artworks/yume_no_uchi/3.jpg'] , tags: '少女, VOCALOID, 幻想, 帅气, 动态, 色彩, 华丽'},
    { name: '上倉エク', avatar: '/nexus/images/artists/uekura_eku.jpg', pixiv: 'https://www.pixiv.net/users/299299', works: ['/nexus/images/artworks/uekura_eku/1.jpg','/nexus/images/artworks/uekura_eku/2.jpg','/nexus/images/artworks/uekura_eku/3.jpg'] , tags: '少女, 可爱, 天使, 双马尾, 色彩, 梦幻, 治愈'},
  ];

const musicItems = [
  { artist: 'Vicetone_Cozi Zuehlsdorff', song: 'Nevada' },
  { artist: 'Vicetone_Kat Nestel', song: 'Angels (Extended Mix)' },
  { artist: 'Vicetone_Kat Nestel', song: 'Angels (Radio Edit)' },
  { artist: 'Various Artists', song: 'sunflower feelings' },
  { artist: 'Vengaboys', song: 'Boom, Boom, Boom, Boom!!' },
  { artist: 'Vicetone _ Cozi Zuehlsdorff', song: 'Way Back (feat_ Cozi Zuehlsdorff)' },
  { artist: 'WAX', song: '졸려 (困了)' },
  { artist: 'Whitney Houston', song: 'Call You Tonight' },
  { artist: 'Whitney Houston', song: 'I Will Always Love You (Film Version)' },
  { artist: 'waera', song: 'harinezumi' },
  { artist: 'Walk off the Earth', song: 'Summer Vibe (Album Version)' },
  { artist: 'Walk off the Earth_D Smoke', song: 'Bet On Me' },
  { artist: 'Two Steps From Hell_Thomas Bergersen', song: 'Victory' },
  { artist: 'TypeD', song: 'Faded Light (Remix)' },
  { artist: 'TYSM', song: 'Normal No More' },
  { artist: 'Tungevaag', song: 'Wicked Wonderland' },
  { artist: 'Tungevaag _ Raaban', song: 'Samsara' },
  { artist: 'Twins', song: '下一站天后 (合唱版)' },
  { artist: 'Usher_Lil Jon_Ludacris', song: 'Yeah' },
  { artist: 'Valentin (ヴァレンティン)', song: 'Unchanged Mind' },
  { artist: 'Various Artists', song: 'Duvet' },
  { artist: 'UMI', song: 'Breathe' },
  { artist: 'UMI', song: 'Pretty Girl hi!' },
  { artist: 'Unlike Pluto_Mike Taylor', song: 'Everything Black' },
  { artist: 'Yung Bae_bbno$_Billy Marchiafava', song: 'Bad Boy' },
  { artist: 'Yung Bleu', song: 'Ice On My Baby' },
  { artist: 'Yunomi (ゆのみ)_nicamoq (にかもきゅ)', song: 'インドア系ならトラックメイカー (内向都是作曲家)' },
  { artist: 'YOIRUI', song: 'uniqueness' },
  { artist: 'Yolanda Be Cool_Dcup', song: 'We No Speak Americano (UK Radio Edit)' },
  { artist: 'Young God_Matt Diesel', song: 'Sleepwalker' },
  { artist: 'Zella Day', song: 'East of Eden' },
  { artist: 'Zyboy忠宇', song: '堕' },
  { artist: 'Zyboy忠宇', song: '妈妈的话' },
  { artist: 'Zac Poor', song: 'Green Light (男生版)' },
  { artist: 'Zedd_Alessia Cara', song: 'Stay' },
  { artist: 'Zedd_Jon Bellion', song: 'Beautiful Now' },
  { artist: 'Wonder Girls (원더걸스)', song: 'Tell Me' },
  { artist: 'Wonder Girls', song: 'Nobody (Nobody ～眼里只有你～) (korean Ver_)' },
  { artist: 'Wwd_J', song: 'Right Above It (Wwd_J改编版)' },
  { artist: 'Wiener Philharmoniker_Carlos Kleiber', song: 'Symphony No_ 5 in C Minor, Op_ 67 - I_ Allegro con brio (c小调第五号交响曲， Op_ 67 - 第一乐章 有活力的快板)' },
  { artist: 'Wild Cards _ James Delaney', song: 'Falling Down (feat_ James Delaney)' },
  { artist: 'Wlstka', song: '心做 (Wlstka remix)' },
  { artist: 'Yael Naïm', song: 'New Soul' },
  { artist: 'YOASOBI (ヨアソビ)', song: '群青' },
  { artist: 'YOASOBI', song: 'たぶん (大概)' },
  { artist: 'XXXCA', song: 'Do It (OrginalMix)' },
  { artist: 'XXXTentacion', song: 'Everybody Dies In Their Nightmares' },
  { artist: 'XXXTentacion', song: 'whoa (mind in awe)' },
  { artist: 'The Score', song: 'Legend' },
  { artist: 'The Tango Project', song: 'Por Una Cabeza' },
  { artist: 'The Walters', song: 'I Love You So' },
  { artist: 'The Harmonics', song: 'The Other Side of Paradise' },
  { artist: 'The Kid LAROI_Justin Bieber', song: 'STAY (Clean)' },
  { artist: 'The Kid LAROI_Justin Bieber', song: 'STAY' },
  { artist: 'The Weeknd', song: 'Earned It (From The _Fifty Shades Of Grey_ Soundtrack)' },
  { artist: 'The Weeknd', song: 'Reminder' },
  { artist: 'The Weeknd_Daft Punk', song: 'Starboy' },
  { artist: 'The Weeknd', song: 'A Lonely Night' },
  { artist: 'The Weeknd', song: 'After Hours' },
  { artist: 'The Weeknd', song: 'Die For You' },
  { artist: 'Tennis', song: 'Im Callin' },
  { artist: 'Terror Jr', song: '3 Strikes' },
  { artist: 'Teyana Taylor', song: 'Gonna Love Me' },
  { artist: 'TAEYANG (太阳)', song: '눈,코,입 (眼，鼻，嘴)' },
  { artist: 'TaIsil', song: 'Hate Me (TaIsil remix)' },
  { artist: 'Tanir & Tyomcha', song: 'Da Da Da (Remix by Mikis)' },
  { artist: 'The Beatles', song: 'Let It Be (Remastered 2009)' },
  { artist: 'The Beatles', song: 'Ob-La-Di, Ob-La-Da (Remastered 2009)' },
  { artist: 'The Chainsmokers_Halsey', song: 'Closer' },
  { artist: 'Tez Cadey', song: 'Seve (Radio Edit)' },
  { artist: 'The Beatles', song: 'Hey Jude (Love Version)' },
  { artist: 'The Beatles', song: 'In My Life (Remastered 2009)' },
  { artist: 'Tsundere Twintails', song: 'A Letter' },
  { artist: 'Tsundere Twintails', song: 'Achoo!' },
  { artist: 'Tsundere Twintails', song: 'Cat Cafe' },
  { artist: 'Tommee Profitt', song: 'O Come O Come Emmanuel' },
  { artist: 'Tommee Profitt _ Fleurie _ Jung Youth', song: 'In The End' },
  { artist: 'Trifect', song: 'Neverland' },
  { artist: 'Tsundere Twintails', song: 'Seashells' },
  { artist: 'Tsundere Twintails', song: 'Snowfall' },
  { artist: 'Tsundere Twintails', song: 'Waiting' },
  { artist: 'Tsundere Twintails', song: 'Cute Circus' },
  { artist: 'Tsundere Twintails', song: 'Head Empty' },
  { artist: 'Tsundere Twintails', song: 'Meet Lime Cookie! (Twintails Edition) (Twintails Edition)' },
  { artist: 'ThimLife_Bibiane Z_Blaze U', song: 'Home (Blaze U Remix)' },
  { artist: 'THT', song: '葬花' },
  { artist: 'Tim King', song: 'River Flows in You' },
  { artist: 'The Weeknd_Gesaffelstein', song: 'I Was Never There' },
  { artist: 'The Weeknd_Playboi Carti_Madonna', song: 'Popular (From The Idol Vol_ 1_Music from the HBO Original Series_Explicit)' },
  { artist: 'ThimLife _ Bibiane Z', song: 'Home (Blaze U Remix Edit)' },
  { artist: 'Tobu', song: 'Life' },
  { artist: 'Tolga Aslan', song: 'SexyBack (Tolga Aslan Remix)' },
  { artist: 'Tomggg (トムグググ)_Raychel Jay', song: 'Away With Me' },
  { artist: 'Timbaland_OneRepublic', song: 'Apologize' },
  { artist: 'TINY7', song: '零距离的思念' },
  { artist: 'TK from 凛冽时雨 (TK from 凛として時雨)', song: 'unravel' },
  { artist: 'Öwnboss _ Sevek', song: 'Move Your Body (RAIZHELL Remix_Razihel Remix_Remix)' },
  { artist: 'アトラスサウンドチーム', song: 'パンチdeアウチ (Punch de Ouch)' },
  { artist: 'アトラスサウンドチーム', song: 'ビッグバン・バーガーのマーチ (Big Bang Burger March)' },
  { artist: 'アトラスサウンドチーム', song: 'プラネタリウム (星象仪)' },
  { artist: 'アトラスサウンドチーム', song: 'クレーンゲーム (抓娃娃机)' },
  { artist: 'アトラスサウンドチーム', song: 'スターフォルネウス (Star Forneus)' },
  { artist: 'アトラスサウンドチーム', song: 'デスティニー・ランド (Destiny Land)' },
  { artist: 'アトラスサウンドチーム', song: '傲慢の崩壊 (傲慢的崩坏)' },
  { artist: 'アトラスサウンドチーム', song: '出会い (相遇)' },
  { artist: 'アトラスサウンドチーム', song: '勝利' },
  { artist: 'アトラスサウンドチーム', song: 'プロゴルファー猿田彦 (职业高尔夫球手 猿田彦)' },
  { artist: 'アトラスサウンドチーム', song: 'メメントス (Mementos)' },
  { artist: 'アトラスサウンドチーム', song: '不穏 (危险)' },
  { artist: 'アトラスサウンドチーム', song: 'Trick (诡计)' },
  { artist: 'アトラスサウンドチーム', song: 'TRIPLE SEVEN' },
  { artist: 'アトラスサウンドチーム', song: 'Wake Up, Get Up, Get Out There -instrumental version-' },
  { artist: 'アトラスサウンドチーム', song: 'Talk' },
  { artist: 'アトラスサウンドチーム', song: 'The Spirit (精神)' },
  { artist: 'アトラスサウンドチーム', song: 'Tokyo Emergency' },
  { artist: 'アトラスサウンドチーム', song: 'おかえりなさいませ!ご主人様♡ (主人！欢迎回来)' },
  { artist: 'アトラスサウンドチーム', song: 'つまらない (无聊)' },
  { artist: 'アトラスサウンドチーム', song: 'はったれ五右衛門 (五右卫门)' },
  { artist: 'アトラスサウンドチーム', song: 'Whats Going On_ (发生什么了？)' },
  { artist: 'アトラスサウンドチーム', song: 'Wicked Plan' },
  { artist: 'アトラスサウンドチーム', song: 'Will Power' },
  { artist: 'アトラスサウンドチーム', song: '次期総理の船路に捧げる即興曲 (献给下届总理航程的即兴曲)' },
  { artist: 'アトラスサウンドチーム', song: '欲望' },
  { artist: 'アトラスサウンドチーム', song: '母のいた日々 (有母亲在的日子)' },
  { artist: 'アトラスサウンドチーム', song: '放送事故' },
  { artist: 'アトラスサウンドチーム', song: '方舟' },
  { artist: 'アトラスサウンドチーム', song: '星と僕らと -piano version- (星星与我们)' },
  { artist: 'アトラスサウンドチーム', song: '王と王妃と奴隷 - another version - (国王与王妃与奴隶)' },
  { artist: 'アトラスサウンドチーム', song: '異世界へ (到往异世界)' },
  { artist: 'アトラスサウンドチーム', song: '疑惑' },
  { artist: 'アトラスサウンドチーム', song: '母のいた日々 -another version- (有母亲在的日子)' },
  { artist: 'アトラスサウンドチーム', song: '灼熱の砂漠を往く (前往灼热的沙漠)' },
  { artist: 'アトラスサウンドチーム', song: '王と王妃と奴隷 (国王与王妃与奴隶)' },
  { artist: 'アトラスサウンドチーム', song: '垢太郎鉄道' },
  { artist: 'アトラスサウンドチーム', song: '夏の日の思い出 (夏日的回忆)' },
  { artist: 'アトラスサウンドチーム', song: '家電量販店 (家电大卖场)' },
  { artist: 'アトラスサウンドチーム', song: '告白_秘密 -piano version-' },
  { artist: 'アトラスサウンドチーム', song: '告白_秘密' },
  { artist: 'アトラスサウンドチーム', song: '回想～暗示' },
  { artist: 'アトラスサウンドチーム', song: '廃人化' },
  { artist: 'アトラスサウンドチーム', song: '憤怒の崩壊 (愤怒的崩坏)' },
  { artist: 'アトラスサウンドチーム', song: '懺悔のお時間 (忏悔的时间)' },
  { artist: 'アトラスサウンドチーム', song: '対峙' },
  { artist: 'アトラスサウンドチーム', song: '尋問室' },
  { artist: 'アトラスサウンドチーム', song: '底知れぬ傲慢 (极其的傲慢)' },
  { artist: 'アトラスサウンドチーム', song: 'Beneath the Mask -rain, instrumental version-' },
  { artist: 'アトラスサウンドチーム', song: 'Betrayer' },
  { artist: 'アトラスサウンドチーム', song: 'Blood of Villain' },
  { artist: 'アトラスサウンドチーム', song: 'Alright' },
  { artist: 'アトラスサウンドチーム', song: 'BAR にゅぅカマー' },
  { artist: 'アトラスサウンドチーム', song: 'Beneath the Mask -instrumental version-' },
  { artist: 'アトラスサウンドチーム', song: 'Butterfly Kiss' },
  { artist: 'アトラスサウンドチーム', song: 'Days of Sisters' },
  { artist: 'アトラスサウンドチーム', song: 'Erosion (腐蚀)' },
  { artist: 'アトラスサウンドチーム', song: 'Blooming Villain' },
  { artist: 'アトラスサウンドチーム', song: 'Break it Down -elp version-' },
  { artist: 'アトラスサウンドチーム', song: 'Break it Down' },
  { artist: 'しばやん( ˙꒳​˙)', song: 'タイニーリトル・アジアンタム (小小的铁线蕨) (Single Version)' },
  { artist: 'しばやん( ˙꒳​˙)', song: '彼と彼女とわたしの話' },
  { artist: 'ずっと真夜中でいいのに。 (永远是深夜有多好｡)', song: 'TAIDADA' },
  { artist: '『ユイカ』_れん (Ren)', song: '好きだから。 (因为我喜欢你。)' },
  { artist: 'さユり (Sayuri)', song: '月と花束 (月与花束)' },
  { artist: 'さユり (Sayuri)', song: '花の塔 (花之塔)' },
  { artist: 'アトラスサウンドチーム', song: 'A Woman' },
  { artist: 'アトラスサウンドチーム', song: 'Alleycat' },
  { artist: 'アトラスサウンドチーム', song: 'Alright -elp version-' },
  { artist: 'ずっと真夜中でいいのに。 (永远是深夜有多好｡)', song: '秒針を噛む (噬咬秒针)' },
  { artist: 'ずっと真夜中でいいのに。', song: 'Dear Mr「F」' },
  { artist: 'アトラスサウンドチーム', song: 'A Woman -another version-' },
  { artist: 'アトラスサウンドチーム', song: 'Regret' },
  { artist: 'アトラスサウンドチーム', song: 'Rivers In the Desert -instrumental version-' },
  { artist: 'アトラスサウンドチーム', song: 'RUN,RUN,RUN!' },
  { artist: 'アトラスサウンドチーム', song: 'Phantom' },
  { artist: 'アトラスサウンドチーム', song: 'Price (代价)' },
  { artist: 'アトラスサウンドチーム', song: 'Price -another version- (代价)' },
  { artist: 'アトラスサウンドチーム', song: 'Sweatshop (血汗工厂)' },
  { artist: 'アトラスサウンドチーム', song: 'Sweatshop -another version- (血汗工厂)' },
  { artist: 'アトラスサウンドチーム', song: 'Sweet' },
  { artist: 'アトラスサウンドチーム', song: 'Sun' },
  { artist: 'アトラスサウンドチーム', song: 'Sunset Bridge' },
  { artist: 'アトラスサウンドチーム', song: 'Swear to My Bones' },
  { artist: 'アトラスサウンドチーム', song: 'Jaldabaoth' },
  { artist: 'アトラスサウンドチーム', song: 'Keeper of Lust' },
  { artist: 'アトラスサウンドチーム', song: 'Layer Cake' },
  { artist: 'アトラスサウンドチーム', song: 'Have a Short Rest' },
  { artist: 'アトラスサウンドチーム', song: 'Hawaii (夏威夷)' },
  { artist: 'アトラスサウンドチーム', song: 'High Pressure' },
  { artist: 'アトラスサウンドチーム', song: 'New Beginning (新的开始)' },
  { artist: 'アトラスサウンドチーム', song: 'Nothingness____' },
  { artist: 'アトラスサウンドチーム', song: 'Our Beginning' },
  { artist: 'アトラスサウンドチーム', song: 'Life Goes On' },
  { artist: 'アトラスサウンドチーム', song: 'Life Will Change -instrumental version-' },
  { artist: 'アトラスサウンドチーム', song: 'My Homie' },
  { artist: 'Queen', song: 'All Dead, All Dead (Remastered 2011)' },
  { artist: 'Queen', song: 'Another One Bites The Dust (Remastered 2011)' },
  { artist: 'Queen', song: 'Crazy Little Thing Called Love' },
  { artist: 'PREP_Tyler Johnson_Kid Harpoon', song: 'As It Was' },
  { artist: 'ProdbyMend', song: 'Billie Jean (Miracle Remix)' },
  { artist: 'Professor Green', song: 'In The Shadow Of The Sun' },
  { artist: 'Queen', song: 'Radio Ga Ga (Remastered 2011)' },
  { artist: 'Radical Face', song: 'Welcome Home' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: 'なんでもないや (没什么大不了) (Movie ver_)' },
  { artist: 'Queen', song: 'Dont Stop Me Now' },
  { artist: 'Queen', song: 'Friends Will Be Friends (Remastered 2011)' },
  { artist: 'Queen', song: 'Keep Passing The Open Windows (Remastered 2011)' },
  { artist: 'Pháo_CM1X', song: 'Hai Phút Hơn (Remix)' },
  { artist: 'Pianoboy高至豪', song: 'The Truth That You Leave' },
  { artist: 'PIKO太郎 (ピコ太郎)', song: 'ペンパイナッポーアッポーペン(PPAP)' },
  { artist: 'Passion Pit', song: 'Take a Walk (Album Version)' },
  { artist: 'Paula DeAnda', song: 'Why Would I Ever' },
  { artist: 'Peter Hurford', song: 'Toccata and Fugue in D minor, BWV 565 - Toccata (托卡塔)' },
  { artist: 'Post Malone_Swae Lee', song: 'Sunflower (Spider-Man_ Into the Spider-Verse)' },
  { artist: 'Powfu_beabadoobee', song: 'death bed (coffee for your head)' },
  { artist: 'PREP_Llywelyn ap Myrddin_Thomas Havelock_Daniel Radclyffe_Guillaume Jambel', song: 'Back To You' },
  { artist: 'Pink Floyd', song: 'The Great Gig in the Sky' },
  { artist: 'pluko _ pronouncedyea _ Egzod', song: 'Feel the Fire (Egzod Remix)' },
  { artist: 'Pogo', song: 'Forget' },
  { artist: 'RADWIMPS', song: 'なんでもないや (没什么大不了) (Movie edit_)' },
  { artist: 'RADWIMPS', song: 'なんでもないや (没什么大不了) (Movie ver_)' },
  { artist: 'RADWIMPS', song: 'はじめての、東京 (初到东京)' },
  { artist: 'RADWIMPS', song: '『天気の子』のテーマ' },
  { artist: 'RADWIMPS', song: 'かたわれ時 (黄昏之时)' },
  { artist: 'RADWIMPS', song: 'そっけない (冷淡)' },
  { artist: 'RADWIMPS', song: 'デート (约会)' },
  { artist: 'RADWIMPS', song: 'デート2 (约会2)' },
  { artist: 'RADWIMPS', song: 'バイクチェイス' },
  { artist: 'RADWIMPS', song: 'ふたたびの、雨 (又下雨了)' },
  { artist: 'RADWIMPS', song: 'ふたりの異変 (两人的异变)' },
  { artist: 'RADWIMPS', song: 'スパークル (火花) (Movie ver_)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '二人の時間 (两人的时间)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '前前前世 (Movie ver_)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '夢灯籠' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: 'スパークル (火花) (Movie ver_)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: 'デート (约会)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '三葉のテーマ (三叶的主题音乐)' },
  { artist: 'RADWIMPS', song: 'K&A 入社式' },
  { artist: 'RADWIMPS', song: 'K&A 初訪問' },
  { artist: 'RADWIMPS', song: 'me me she' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '家族の時間 (家庭时间)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '愛にできることはまだあるかい (爱能做到的还有什么) (Movie edit)' },
  { artist: 'RADWIMPS (ラッドウィンプス)', song: '愛にできることはまだあるかい (爱能做到的还有什么)' },
  { artist: 'Mokenstef', song: 'Hes Mine' },
  { artist: 'Molchat Doma', song: 'Клетка' },
  { artist: 'Mose N_MD Dj', song: 'Sweet Sensation (Extended)' },
  { artist: 'mj apanay _ aren park', song: 'time machine (feat_ aren park)' },
  { artist: 'Modern Talking', song: 'Brother Louie' },
  { artist: 'MOGUAI_Cheat Codes', song: 'Hold On(feat_ Cheat Codes) (2020 Edit)' },
  { artist: 'Murray Perahia _ Radu Lupu _ Wolfgang Amadeus Mozart', song: 'Sonata for 2 Pianos in D Major, K_448_375a - II_ Andante (第二乐章 行板)' },
  { artist: 'Najwa Mahiaddin_Najwa Mahdi', song: 'Someone You Loved' },
  { artist: 'NALYRO', song: 'NALYRO - Lush Life' },
  { artist: 'Mr FijiWiji_AgNO3_Laura Brehm', song: 'Pure Sunlight' },
  { artist: 'Mr_ Belt & Wezol_Jack Wins', song: 'One Thing' },
  { artist: 'Ms_ Lauryn Hill', song: 'Cant Take My Eyes Off of You (I Love You Baby)' },
  { artist: 'matryoshka', song: 'Sacred Play Secret Place' },
  { artist: 'Matteo', song: 'Panama' },
  { artist: 'Mazie', song: 'dumb dumb' },
  { artist: 'Martin Garrix_Troye Sivan', song: 'There For You' },
  { artist: 'Mary J_ Blige', song: 'Mary Jane (All Night Long)' },
  { artist: 'MatisYahu', song: 'One Day' },
  { artist: 'Michael Jackson', song: 'We Are the World (Demo)' },
  { artist: 'Mii Media_Trí Thức Remix', song: 'Nothin on Me (Trí Thức Remix_Explicit)' },
  { artist: 'Mitski', song: 'My Love Mine All Mine' },
  { artist: 'MC梦 (MC몽)_Mellow (멜로우)', song: '죽을 만큼 아파서 (死一样的痛苦)' },
  { artist: 'Meduza_Dermot Kennedy', song: 'Paradise' },
  { artist: 'Michael Calfan_INNA', song: 'Call Me Now' },
  { artist: 'ONE OK ROCK (ワンオクロック)', song: 'I Was King' },
  { artist: 'ONE OK ROCK (ワンオクロック)', song: 'Re_make' },
  { artist: 'ONE OK ROCK (ワンオクロック)', song: 'The Beginning' },
  { artist: 'OMFG', song: 'Hello' },
  { artist: 'One Direction', song: 'What Makes You Beautiful' },
  { artist: 'ONE OK ROCK (ワンオクロック)', song: 'Giants' },
  { artist: 'Papa Tin', song: 'Una Mattina' },
  { artist: 'Parov Stelar', song: 'Libella Swing' },
  { artist: 'Parx_Nonô', song: 'Feel Right Now (feat_ Nonô)' },
  { artist: 'ONE OK ROCK (ワンオクロック)', song: 'We Are' },
  { artist: 'ONE OK ROCK (ワンオクロック)', song: 'Wherever you are' },
  { artist: 'ONE OK ROCK (ワンオクロック)_Kiiara', song: 'In the Stars (feat_ Kiiara)' },
  { artist: 'Nicky', song: '说散就散 (弹唱版)' },
  { artist: 'Nightwish', song: 'Last of the Wilds' },
  { artist: 'nomico (のみこ)', song: 'Bad Apple!! (坏苹果)' },
  { artist: 'Ne', song: 'Yo - Because Of You' },
  { artist: 'Ne', song: 'Yo - So Sick' },
  { artist: 'New Order', song: 'Blue Monday (12_ Version)' },
  { artist: 'Official髭男dism (OFFICIAL HIGE DANDISM)', song: 'Pretender' },
  { artist: 'Oh Wonder', song: 'Landslide' },
  { artist: 'Oliver Tree', song: 'Life Goes On' },
  { artist: 'Norman Brown', song: 'Night Drive' },
  { artist: 'Normani _ Cardi B', song: 'Wild Side' },
  { artist: 'Nujabes (せば じゅん)', song: 'Luv (Sic)' },
  { artist: 'RADWIMPS', song: '三葉のテーマ (三叶的主题音乐)' },
  { artist: 'sapientdream', song: 'WALLS' },
  { artist: 'Sarah Watson', song: 'Growth' },
  { artist: 'Sarah Watson', song: 'Nursery' },
  { artist: 'sakanaction (サカナクション)', song: '新宝島' },
  { artist: 'Sam Smith', song: 'Im Not The Only One' },
  { artist: 'Sand', song: 'China-2' },
  { artist: 'SawanoHiroyuki[nZk] (泽野弘之)_瑞葵 (mizuki)', song: 'Avid' },
  { artist: 'SawanoHiroyuki[nZk] (泽野弘之)_瑞葵 (mizuki)', song: 'Oi' },
  { artist: 'Saweetie _ Jhené Aiko', song: 'Back to the Streets (feat_ Jhené Aiko)' },
  { artist: 'Sassydee', song: 'Sweet but Psycho' },
  { artist: 'SawanoHiroyuki[nZk] (泽野弘之)_R!N_Gemie', song: 'X_U_' },
  { artist: 'SawanoHiroyuki[nZk] (泽野弘之)_瑞葵 (mizuki)', song: 'aLIEz' },
  { artist: 'Rick Astley', song: 'Never Gonna Give You Up' },
  { artist: 'Rival _ Cadmium _ Harley Bird', song: 'Seasons (Futuristik & Whogaux Remix)' },
  { artist: 'Roar', song: 'I Cant Handle Change' },
  { artist: 'Reynard Silva', song: 'The Way I Still Love You' },
  { artist: 'Richie Kotzen', song: 'special' },
  { artist: 'Ricii Lompeurs _ Ticia', song: 'PLAY' },
  { artist: 'Russ', song: 'Psycho,Pt_ 2 (Pt_ 2_Explicit)' },
  { artist: 'SAFIA', song: 'Counting Sheep' },
  { artist: 'Saib_', song: 'Battlecry (Saib_ Mix)' },
  { artist: 'Robin Hustin _ TobiMorrow _ Jex', song: 'Light It Up' },
  { artist: 'Romi Kopelman', song: 'Gymnopédie no_ 1' },
  { artist: 'Ronny Jordan _ Fay Simpson', song: 'Keep Your Head Up' },
  { artist: 'suhmeduh', song: 'Phone Kisses' },
  { artist: 'Suki刘舒妤', song: '失眠' },
  { artist: 'sumimi', song: 'Here, the world!' },
  { artist: 'Sound Souler', song: 'Paradise' },
  { artist: 'Steve James_Clairity', song: 'Renaissance' },
  { artist: 'Strangerboi24_Nightcore_Murkish_joseph rodgigo', song: 'Wasted (Murkish Remix_Explicit)' },
  { artist: 'SZA', song: 'Good Days' },
  { artist: 'SZA _ Calvin Harris', song: 'The Weekend (Funk Wav Remix)' },
  { artist: 'T', song: 'ara - Falling U (爱上你)' },
  { artist: 'sunflow3rs', song: 'i watch the moon' },
  { artist: 'Sunstroke Project', song: 'Hey Mamma' },
  { artist: 'Synth', song: '24_7 (Single Version)' },
  { artist: 'Shin Giwon Piano', song: 'Hush' },
  { artist: 'Sia', song: 'Cheap Thrills' },
  { artist: 'Sia _ Alan Walker', song: 'Move Your Body (Alan Walker Remix)' },
  { artist: 'SeVen_13', song: 'Limitless (无限)' },
  { artist: 'SFDK', song: 'Ternera Podrida (Inst_)' },
  { artist: 'Shakira_Freshlyground', song: 'Waka Waka (This Time for Africa)' },
  { artist: 'Slushii', song: 'Past Lives' },
  { artist: 'SoMo', song: '50 Feet' },
  { artist: 'Soriano', song: 'Old School' },
  { artist: 'Sia _ Alan Walker', song: 'Move Your Body (Alan Walker Remix)_hires' },
  { artist: 'SIEサウンドチーム', song: 'ロゼの黄昏 (罗斯的黄昏)' },
  { artist: 'SING女团', song: '123木头人' },
  { artist: 'RADWIMPS', song: '御宅訪問' },
  { artist: 'RADWIMPS', song: '御神体' },
  { artist: 'RADWIMPS', song: '御神体へ再び (重返神体)' },
  { artist: 'RADWIMPS', song: '奥寺先輩のテーマ (奥寺前辈的主题音乐)' },
  { artist: 'RADWIMPS', song: '家族の時間 (家庭时间)' },
  { artist: 'RADWIMPS', song: '帆高、逃走 ~子供達の画策' },
  { artist: 'RADWIMPS', song: '旅館の夜 (旅馆之夜)' },
  { artist: 'RADWIMPS', song: '晴れゆく空 (晴朗的天空)' },
  { artist: 'RADWIMPS', song: '晴天と喪失' },
  { artist: 'RADWIMPS', song: '愛にできることはまだあるかい (爱能做到的还有什么) (Movie edit)' },
  { artist: 'RADWIMPS', song: '愛にできることはまだあるかい (爱能做到的还有什么)' },
  { artist: 'RADWIMPS', song: '憧れカフェ (憧憬的咖啡馆)' },
  { artist: 'RADWIMPS', song: '優しさの味' },
  { artist: 'RADWIMPS', song: '初の晴れ女バイト' },
  { artist: 'RADWIMPS', song: '前前前世 (Movie ver_)' },
  { artist: 'RADWIMPS', song: '三葉の通学 (上学的三叶)' },
  { artist: 'RADWIMPS', song: '二つの告白' },
  { artist: 'RADWIMPS', song: '作戦会議 (Council Of War)' },
  { artist: 'RADWIMPS', song: '夢灯籠' },
  { artist: 'RADWIMPS', song: '大丈夫 (Movie edit)' },
  { artist: 'RADWIMPS', song: '天気の力' },
  { artist: 'RADWIMPS', song: '占秘館へようこそ' },
  { artist: 'RADWIMPS', song: '口噛み酒トリップ (口嚼酒TRIP)' },
  { artist: 'RADWIMPS', song: '図書館' },
  { artist: 'RADWIMPS', song: '風たちの声 (风声) (Movie edit)' },
  { artist: 'RADWIMPS', song: '飛騨探訪 (Visit To Hida)' },
  { artist: 'RADWIMPS', song: '首都危機' },
  { artist: 'RADWIMPS', song: '記憶を呼び起こす瀧 (唤醒记忆的泷)' },
  { artist: 'RADWIMPS', song: '陽菜、救出' },
  { artist: 'RADWIMPS', song: '陽菜と、走る帆高' },
  { artist: 'Rameses B_Anna Yvette', song: 'Neon Rainbow' },
  { artist: 'Re', song: 'Logic - Overworld Day' },
  { artist: 'Rentz', song: 'Alone' },
  { artist: 'Raimu', song: 'Botanicals' },
  { artist: 'Rainych', song: 'Nyanpasu' },
  { artist: 'Rameses B _ Anna Yvette', song: 'Neon Rainbow' },
  { artist: 'RADWIMPS', song: '消えゆく陽菜' },
  { artist: 'RADWIMPS', song: '町長説得 (劝说镇长)' },
  { artist: 'RADWIMPS', song: '真夏の雪' },
  { artist: 'RADWIMPS', song: '気象神社' },
  { artist: 'RADWIMPS', song: '永遠の雲の上' },
  { artist: 'RADWIMPS', song: '消えた町 (消失的城镇)' },
  { artist: 'RADWIMPS', song: '芝公園' },
  { artist: 'RADWIMPS', song: '花火大会' },
  { artist: 'RADWIMPS', song: '見えないふたり (无法看见彼此)' },
  { artist: 'RADWIMPS', song: '秋祭り (秋日祭)' },
  { artist: 'RADWIMPS', song: '空の海' },
  { artist: 'RADWIMPS', song: '糸守高校' },
  { artist: 'アトラスサウンドチーム', song: '終わらない日々 (无尽的日子)' },
  { artist: '王杰', song: '心瘾' },
  { artist: '王杰', song: '我是真的爱上你' },
  { artist: '王梓钰', song: '青春不打烊' },
  { artist: '王忻辰_苏星婕', song: '清空' },
  { artist: '王杰', song: '一场游戏一场梦' },
  { artist: '王杰', song: '不浪漫罪名' },
  { artist: '王雨桐', song: '光的方向 (弹唱版)' },
  { artist: '生物股长', song: 'HANABI (Album Version)' },
  { artist: '田中あいみ (田中爱美)', song: 'かくしん的☆めたまるふぉ~ぜっ! (革新的变身)' },
  { artist: '王艳薇', song: '离开我的依赖' },
  { artist: '王菲', song: '传奇' },
  { artist: '王蓝茵', song: '恶作剧' },
  { artist: '王以太_刘至佳', song: '危险派对' },
  { artist: '王力宏', song: '唯一' },
  { artist: '王力宏', song: '大城小爱' },
  { artist: '片岡知子_マニュアル・オブ・エラーズ', song: 'たのしいたまこ・ピアノソロ' },
  { artist: '牛尾憲輔 (agraph)', song: 'Night Cruising (夜间巡航)' },
  { artist: '物語シリーズ', song: '恋愛サーキュレーション (恋爱循环)' },
  { artist: '王心凌', song: '大眠' },
  { artist: '王心凌', song: '爱你' },
  { artist: '王心凌', song: '第一次爱的人' },
  { artist: '王力宏', song: '我们的歌' },
  { artist: '王力宏', song: '改变自己' },
  { artist: '王力宏', song: '爱错' },
  { artist: '罗百吉 _ 宝贝', song: 'I Miss You' },
  { artist: '胡夏', song: '那些年' },
  { artist: '胡彦斌', song: 'One Night In Shanghai' },
  { artist: '緑黄色社会', song: 'ずっとずっとずっと (永远，永远，永远)' },
  { artist: '约瑟翰 庞麦郎', song: '我的滑板鞋' },
  { artist: '纯音乐', song: 'Flower Dance (花之舞) (钢琴版)' },
  { artist: '苏打绿', song: '我好想你 (苏打绿版)' },
  { artist: '茅野愛衣 _ 戸松遥 _ 早見沙織', song: 'secret base～君がくれたもの～ (secret base ~你给我的所有~) (10 years after Ver_)' },
  { artist: '茜拉 (Shila Amzah)', song: '到时说爱我' },
  { artist: '花澤香菜 (はなざわ かな)', song: '恋愛サーキュレーション (恋爱循环)' },
  { artist: '花澤香菜', song: 'magical mode' },
  { artist: '苏打绿', song: '小情歌 (苏打绿版)' },
  { artist: '程响', song: '四季予你' },
  { artist: '立秋_初音未来 (初音ミク)', song: 'うに (海胆)' },
  { artist: '竹达彩奈 (たけたつ あやな)', song: 'Joyful Todays' },
  { artist: '田中あいみ (田中爱美)', song: 'かくしん的☆めたまるふぉ～ぜっ！ (革新的变身)' },
  { artist: '田馥甄', song: '你就不要想起我' },
  { artist: '石进', song: '夜的钢琴曲五' },
  { artist: '米津玄師 (よねづ けんし)', song: '海の幽霊 (海之幽灵)' },
  { artist: '米津玄師', song: 'Flamingo' },
  { artist: '結束バンド (纽带乐队)', song: 'ラブソングが歌えない (不会唱情歌)' },
  { artist: '米津玄師 (よねづ けんし)', song: 'Lemon' },
  { artist: '米津玄師 (よねづ けんし)', song: 'LOSER' },
  { artist: '米津玄師 (よねづ けんし)', song: '打上花火 (烟火绽放)' },
  { artist: '梁静茹', song: '用力抱着' },
  { artist: '梁静茹_曹格', song: 'PK' },
  { artist: '梦飞船', song: '不值得' },
  { artist: '梁静茹', song: '天灯' },
  { artist: '梁静茹', song: '失忆' },
  { artist: '梁静茹', song: '情歌' },
  { artist: '橘子海', song: '夏日漱石' },
  { artist: '欧阳耀莹', song: '戒不掉 (原声版)' },
  { artist: '比莉', song: 'Dear John' },
  { artist: '森永千才 (もりなが ちとせ)', song: 'ミチノチモシーキミノキモチ (路上的猫尾草 你的心情)' },
  { artist: '椎名林檎', song: '17' },
  { artist: '椎名林檎', song: '茜さす 帰路照らされど…' },
  { artist: '林俊杰', song: '曹操' },
  { artist: '林俊杰', song: '江南' },
  { artist: '林俊杰', song: '背对背拥抱' },
  { artist: '林俊杰', song: 'Always Online' },
  { artist: '林俊杰', song: '不潮不用花钱' },
  { artist: '林俊杰', song: '新地球' },
  { artist: '林海', song: '欢沁' },
  { artist: '梁静茹', song: '勇气' },
  { artist: '梁静茹', song: '可惜不是你' },
  { artist: '林子祥', song: '敢爱敢做' },
  { artist: '林忆莲', song: '呼吸' },
  { artist: '林忆莲', song: '至少还有你' },
  { artist: '澤野弘之 (さわの ひろゆき)', song: 'REVIVƎЯ' },
  { artist: '澤野弘之 (さわの ひろゆき)_小林未郁 (こばやし みか)', song: 'βios' },
  { artist: '澤野弘之 _ SennaRin', song: 'IVORY TOWER (feat_ SennaRin)' },
  { artist: '游戏科学_8082Audio', song: '我也去当个天命人玩玩' },
  { artist: '潘玮柏_弦子', song: '不得不爱' },
  { artist: '澤野弘之 (さわの ひろゆき)', song: 'Call of Silence' },
  { artist: '片岡知子 _ マニュアル・オブ・エラーズ', song: 'もっと楽しいたまこ' },
  { artist: '片岡知子 _ マニュアル・オブ・エラーズ', song: 'もの思いのたまこ' },
  { artist: '片岡知子 _ マニュアル・オブ・エラーズ', song: '恋のあれこれ' },
  { artist: '濯', song: '我好想你 (架子鼓版)' },
  { artist: '片岡知子 _ マニュアル・オブ・エラーズ', song: 'おセンチたまこ (悲伤的蛋)' },
  { artist: '片岡知子 _ マニュアル・オブ・エラーズ', song: 'たのしいたまこ' },
  { artist: '江语晨', song: '最后一页' },
  { artist: '池鱼', song: '天若有情 (深情版)' },
  { artist: '泳儿', song: '惯坏' },
  { artist: '毛毛不插电', song: '天生反骨鸵鸟' },
  { artist: '水濑祈 (水瀬いのり) _ 久保由利香', song: 'More One Night (试听版)' },
  { artist: '永彬Ryan_B', song: '像极了' },
  { artist: '渡边雅二', song: '青山绿野' },
  { artist: '温岚', song: '你活该' },
  { artist: '温岚', song: '冰河' },
  { artist: '泽亦龙', song: 'Tuesday (DJ泽亦龙版)' },
  { artist: '浅影阿_汐音社', song: '探故知' },
  { artist: '海伦', song: '游山恋' },
  { artist: '草东没有派对', song: '但' },
  { artist: '陈奕迅', song: '最佳损友' },
  { artist: '陈奕迅', song: '浮夸' },
  { artist: '陈奕迅', song: '淘汰' },
  { artist: '陈奕迅', song: '富士山下' },
  { artist: '陈奕迅', song: '想哭' },
  { artist: '陈奕迅', song: '昨日' },
  { artist: '陈小春', song: '乱世巨星' },
  { artist: '陈慧娴', song: '傻女 (Album Version)' },
  { artist: '陈慧琳', song: '不如跳舞' },
  { artist: '陈奕迅', song: '稳稳的幸福' },
  { artist: '陈奕迅', song: '落花流水' },
  { artist: '陈奕迅', song: '葡萄成熟时' },
  { artist: '阿禹ayy', song: '耍把戏' },
  { artist: '陈冠希_MC仁_厨房仔_应采儿', song: 'Everywhere We Go' },
  { artist: '陈奕迅', song: 'Shall We Talk' },
  { artist: '闻人听書_', song: '一笑江湖 (DJ弹鼓版)' },
  { artist: '闻人听書_', song: '美人画卷' },
  { artist: '阿禹ayy', song: '一吻天荒 (热血版)' },
  { artist: '陈奕迅', song: '十面埋伏' },
  { artist: '陈奕迅', song: '单车' },
  { artist: '陈奕迅', song: '孤独患者' },
  { artist: '陈奕迅', song: '不要说话' },
  { artist: '陈奕迅', song: '你的背包' },
  { artist: '陈奕迅', song: '十年' },
  { artist: '高梨康治 _ 刃', song: 'yaiba- - 五月雨' },
  { artist: '鷲尾伶菜', song: 'Photogenic Boy' },
  { artist: '黄凯芹', song: '若生命等候' },
  { artist: '韦礼安', song: '如果可以' },
  { artist: '韩安旭', song: '多幸运' },
  { artist: '风花似雪月', song: 'Forest Mixtape' },
  { artist: '브라운아이드걸스', song: 'Abracadabra' },
  { artist: '피아노자장가', song: '肖邦：夜曲 (Chopin_ Nocturne, Op_ 9, No_ 2)' },
  { artist: '허회경', song: '그렇게 살아가는 것 (So life goes on) (就这样活着)' },
  { artist: '齐秦', song: '大约在冬季' },
  { artist: '龙井说唱', song: '归' },
  { artist: '림킴 (김예림)', song: '잘 알지도 못하면서 (什么也不懂)' },
  { artist: '陈绮贞', song: 'After 17' },
  { artist: '陈绮贞', song: '太聪明' },
  { artist: '陈越龙', song: 'nop' },
  { artist: '陈柏宇', song: '你瞒我瞒' },
  { artist: '陈洁丽', song: '猪猪侠' },
  { artist: '陈粒', song: '走马' },
  { artist: '陶喆', song: '爱我还是他' },
  { artist: '陶喆', song: '爱，很简单' },
  { artist: '零一九零贰', song: '忘川彼岸 (DJ名龙版)' },
  { artist: '陈雪凝', song: '你的酒馆对我打了烊' },
  { artist: '陈雪凝', song: '绿色' },
  { artist: '陶喆', song: '天天' },
  { artist: '蔡健雅', song: '越来越不懂' },
  { artist: '蔡健雅', song: '达尔文 II (进化版)' },
  { artist: '蔡健雅', song: '达尔文' },
  { artist: '蔡健雅', song: '下一次爱情来的时候' },
  { artist: '蔡健雅', song: '晨间新闻' },
  { artist: '蔡健雅', song: '红色高跟鞋' },
  { artist: '薛凯琪', song: '慕容雪' },
  { artist: '薛明媛 _ 朱贺', song: '非酋' },
  { artist: '街道办GDC_欧阳耀莹', song: '春娇与志明' },
  { artist: '薛之谦', song: '丑八怪' },
  { artist: '薛之谦', song: '意外' },
  { artist: '薛之谦', song: '认真的雪' },
  { artist: '菅野祐悟', song: '美春の告白 (美春的告白)' },
  { artist: '葛谷葉子', song: 'TRUE LIES' },
  { artist: '董六六', song: 'Glow Me (0_9版)' },
  { artist: '草东没有派对', song: '山海' },
  { artist: '草东没有派对', song: '烂泥' },
  { artist: '草蜢', song: '半点心' },
  { artist: '蔡依林', song: '倒带' },
  { artist: '蔡依林', song: '妥协' },
  { artist: '蔡健雅', song: 'Letting Go' },
  { artist: '蒋雪儿Snow_J', song: '燕无歇' },
  { artist: '蒋雪儿Snow_J', song: '莫问归期' },
  { artist: '蒋雪儿Snow_J', song: '落了白' },
  { artist: '郑润泽', song: '瞬' },
  { artist: '郑鱼', song: '怎叹' },
  { artist: '郭采洁', song: '你在，不在' },
  { artist: '邓寓君(等什么君)', song: '鸳鸯戏' },
  { artist: '邓寓君(等什么君)_FOX胡天渝', song: '踏雪' },
  { artist: '那艺娜', song: '爱如火' },
  { artist: '郭顶', song: '我们俩' },
  { artist: '金海心', song: '阳光下的星星' },
  { artist: '长谷川大祐 (DAISUKE HASEGAWA)', song: 'canzoni preferite' },
  { artist: '郭静', song: '下一个天亮' },
  { artist: '郭静', song: '心墙' },
  { artist: '郭静', song: '爱情讯息' },
  { artist: '许慧欣', song: '七月七日晴' },
  { artist: '谢安琪', song: '囍帖街' },
  { artist: '谭咏麟', song: '讲不出再见' },
  { artist: '言瑾羽', song: '未必' },
  { artist: '许嵩', song: '雅俗共赏' },
  { artist: '许嵩 _ 何曼婷', song: '素颜' },
  { artist: '邓寓君(等什么君)', song: '春庭雪' },
  { artist: '邓寓君(等什么君)', song: '赤伶' },
  { artist: '邓寓君(等什么君)', song: '辞九门回忆' },
  { artist: '费玉清', song: '一剪梅' },
  { artist: '邓寓君(等什么君)', song: '关山酒 (DJ版)' },
  { artist: '邓寓君(等什么君)', song: '同舟' },
  { artist: '周杰伦', song: '周大侠' },
  { artist: '周杰伦', song: '回到过去' },
  { artist: '周杰伦', song: '安静' },
  { artist: '周杰伦', song: '借口' },
  { artist: '周杰伦', song: '兰亭序' },
  { artist: '周杰伦', song: '反方向的钟' },
  { artist: '周杰伦', song: '最长的电影' },
  { artist: '周杰伦', song: '爱在西元前' },
  { artist: '周杰伦', song: '牛仔很忙' },
  { artist: '周杰伦', song: '搁浅' },
  { artist: '周杰伦', song: '明明就' },
  { artist: '周杰伦', song: '晴天' },
  { artist: '印子月', song: '落空' },
  { artist: '原田瞳 _ 茅野愛衣 _ 小仓唯', song: '回レ! 雪月花 (旋转吧!雪月花)' },
  { artist: '古巨基', song: '必杀技' },
  { artist: '卫兰', song: '她整晚在写信' },
  { artist: '卫兰', song: '心乱如麻' },
  { artist: '卫兰', song: '拍错拖' },
  { artist: '周杰伦', song: '一路向北' },
  { artist: '周杰伦', song: '你听得到' },
  { artist: '周杰伦', song: '倒带 (Live)' },
  { artist: '告五人', song: '爱人错过' },
  { artist: '告五人', song: '给你一瓶魔法药水' },
  { artist: '周公', song: '不说' },
  { artist: '堤博明', song: 'Wake Up, Senku!!' },
  { artist: '大壮', song: '差一步' },
  { artist: '大壮', song: '我们不一样' },
  { artist: '喻言', song: '蠢货' },
  { artist: '国风新语_浮生梦_汐音社', song: '探窗' },
  { artist: '坂本龙一', song: 'Merry Christmas Mr_ Lawrence (劳伦斯先生圣诞快乐_圣诞快乐，劳伦斯先生) (Inst_)' },
  { artist: '孙燕姿', song: '害怕' },
  { artist: '孙燕姿', song: '雨天' },
  { artist: '宇多田光 (宇多田ヒカル)', song: 'First Love' },
  { artist: '奶葵', song: '路的尽头好像还是路' },
  { artist: '姚远', song: '卡农 (钢琴版)' },
  { artist: '孙燕姿', song: '安宁' },
  { artist: '周杰伦', song: '蒲公英的约定' },
  { artist: '周杰伦', song: '说了再见' },
  { artist: '周杰伦', song: '说好的幸福呢' },
  { artist: '周杰伦', song: '稻香' },
  { artist: '周杰伦', song: '给我一首歌的时间' },
  { artist: '周杰伦', song: '花海' },
  { artist: '周杰伦', song: '龙卷风' },
  { artist: '周杰伦 _ Lara梁心颐', song: '珊瑚海' },
  { artist: '周杰伦 _ 费玉清', song: '千里之外' },
  { artist: '周杰伦', song: '路小雨' },
  { artist: '周杰伦', song: '退后' },
  { artist: '周杰伦', song: '青花瓷' },
  { artist: 'ヨルシカ (Yorushika)', song: '花に亡霊 (花上的亡灵)' },
  { artist: 'ヨルシカ', song: '嘘月' },
  { artist: 'ラブリーサマーちゃん _ 泉まくら', song: '202 (202) (New Mix)' },
  { artist: 'トゲナシトゲアリ (TOGENASHI TOGEARI)', song: '運命の華 (命运之花)' },
  { artist: 'トゲナシトゲアリ', song: '空の箱 (井芹仁菜、河原木桃香)' },
  { artist: 'トゲナシトゲアリ', song: '雑踏、僕らの街 (熙熙攘攘我们的城市)' },
  { artist: '三叔说', song: '海市蜃楼' },
  { artist: '三楠_周林枫_L（桃籽）', song: '囍与悲 (吉他版)' },
  { artist: '三浦透子', song: '祝祭 (Movie edit)' },
  { artist: '一颗狼星', song: '海市蜃楼 (女声版)' },
  { artist: '丁当', song: '猜不透' },
  { artist: '七朵组合', song: '咏春' },
  { artist: 'アトラスサウンドチーム', song: '自由と安心 (自由与安心)' },
  { artist: 'アトラスサウンドチーム', song: '色欲の崩壊 (色欲的崩坏)' },
  { artist: 'アトラスサウンドチーム', song: '覚醒' },
  { artist: 'アトラスサウンドチーム', song: '緊迫' },
  { artist: 'アトラスサウンドチーム', song: '脱出 (逃离)' },
  { artist: 'アトラスサウンドチーム', song: '脱出 -another version-' },
  { artist: 'アトラスサウンドチーム', song: '闇ネットたなか (在黑暗的网络中)' },
  { artist: 'チマメ隊', song: 'ときめきポポロン♪ (心动泡芙♪)' },
  { artist: 'トゲナシトゲアリ (TOGENASHI TOGEARI)', song: '空白とカタルシス (空白与净化)' },
  { artist: 'アトラスサウンドチーム', song: '豪血寺一味' },
  { artist: 'アトラスサウンドチーム', song: '逃走 -another version-' },
  { artist: 'アトラスサウンドチーム', song: '逃走～逮捕' },
  { artist: '刘若英', song: '后来' },
  { artist: '初音未来 (初音ミク)_Otomania', song: 'Ievan Polkka' },
  { artist: '励阳_徐君', song: '我知道你' },
  { artist: '具島直子', song: 'まどろみ (小睡)' },
  { artist: '具島直子', song: 'モノクローム' },
  { artist: '出羽良彰', song: '自由の時間' },
  { artist: '卢广仲', song: '几分之几' },
  { artist: '卫兰', song: '一格格' },
  { artist: '卫兰', song: '大哥' },
  { artist: '千百顺', song: '很任性' },
  { artist: '南征北战NZBZ', song: '我的天空' },
  { artist: '南社长', song: 'What Are Words (俄式比心)' },
  { artist: '不是花火呀', song: 'Ring Ring Ring' },
  { artist: '中村千尋', song: 'カサネテク (千层套路)' },
  { artist: '中村由利子 (なかむら ゆりこ)', song: 'My Memory (冬季恋歌)' },
  { artist: '三浦透子 _ RADWIMPS', song: 'グランドエスケープ (逃离地面) (Movie edit)' },
  { artist: '三轮学', song: '素直と不器用と意地っ張り (坦率、笨拙与固执)' },
  { artist: '不只中二', song: '牵丝戏 (新版)' },
  { artist: '何洁', song: '你是我的风景' },
  { artist: '何耀珊', song: '握紧' },
  { artist: '光良', song: '童话' },
  { artist: '久石让', song: 'The Rain (映画「菊次郎の夏」より)' },
  { artist: '云狗蛋', song: '天若有情' },
  { artist: '伊格赛听_叶里', song: '谪仙' },
  { artist: '宇多田光 (宇多田ヒカル)', song: 'One Last Kiss (最后一吻)' },
  { artist: '方大同', song: '放不过自己' },
  { artist: '方大同', song: '爱在' },
  { artist: '方大同', song: '特别的人' },
  { artist: '方大同', song: '偷笑' },
  { artist: '方大同', song: '够不够' },
  { artist: '方大同', song: '好不容易' },
  { artist: '时光胶囊', song: '青丝' },
  { artist: '星野源 (ほしの げん)', song: '喜劇' },
  { artist: '晨星', song: '每当我' },
  { artist: '方大同', song: '麦恩莉' },
  { artist: '方大同 _ 葛仲珊', song: '爱不来 (feat_ Miss Ko葛仲珊)' },
  { artist: '日笠阳子 (ひかさ ようこ)', song: 'Heart Goes Boom!!' },
  { artist: '指尖笑', song: '不问ciaga (不问别离)' },
  { artist: '新乐尘符', song: '123我爱你' },
  { artist: '新雅室内乐', song: '花之圆舞曲' },
  { artist: '我是小力气', song: '一格格' },
  { artist: '戚薇', song: '如果爱忘了' },
  { artist: '所长sama', song: '审判时刻' },
  { artist: '方大同', song: 'Singalongsong' },
  { artist: '方大同', song: 'Sorry' },
  { artist: '方大同', song: '为你写的歌' },
  { artist: '方大同', song: '1234567' },
  { artist: '方大同', song: 'BB88' },
  { artist: '方大同', song: 'Love Song' },
  { artist: '李荣浩', song: '李白' },
  { artist: '李荣浩', song: '模特' },
  { artist: '李荣浩', song: '麻雀' },
  { artist: '李荣浩', song: '太坦白' },
  { artist: '李荣浩', song: '小黄' },
  { artist: '李荣浩', song: '恋人' },
  { artist: '松原みき (まつばら みき)', song: '真夜中のドア_Stay With Me (深夜之门) (シングルver_)' },
  { artist: '林依晨', song: '你 (吉他甜蜜版)' },
  { artist: '林俊呈', song: '东西' },
  { artist: '杨丞琳', song: '带我走' },
  { artist: '杨丞琳', song: '怕' },
  { artist: '杨丞琳', song: '雨爱' },
  { artist: '有華', song: 'Baby you (Single Version)' },
  { artist: '末吉秀太', song: 'HACK' },
  { artist: '末廣健一郎', song: '星の願い' },
  { artist: '曹方', song: '遇见我' },
  { artist: '曾惜', song: '讲真的' },
  { artist: '曾沛慈', song: '一个人想着一个人' },
  { artist: '李克勤', song: '红日 (粤语)' },
  { artist: '李宇春', song: '下个，路口，见' },
  { artist: '李玉刚', song: '刚好遇见你' },
  { artist: '朱主爱', song: '好想你' },
  { artist: '朴树', song: '平凡之路' },
  { artist: '李克勤', song: '护花使者' },
  { artist: '尹昔眠_小田音乐社', song: '西楼别序' },
  { artist: '山本希望', song: 'アップルティーの味' },
  { artist: '岑宁儿', song: '追光者' },
  { artist: '小宮知子 _ アトラスサウンドチーム', song: '全ての人の魂の詩 (全人类的灵魂之诗)' },
  { artist: '小淅儿 _ 汐音社', song: '空台戏' },
  { artist: '小霞', song: '拆穿' },
  { artist: '市川淳', song: 'その日の事' },
  { artist: '幸子小姐拜托了', song: '柴 鱼 の c a l l i n g' },
  { artist: '幾田りら (ikura)', song: 'ハミング (哼唱)' },
  { artist: '川青_Morerare', song: '下潜' },
  { artist: '市川淳', song: '-記憶- (记忆) (ヨスガノソラ メインテーマ)' },
  { artist: '市川淳', song: '-遠い空へ- (飞向遥远的天空) (ヨスガノソラ メインテーマ)' },
  { artist: '家家', song: '命运' },
  { artist: '容祖儿', song: '习惯失恋' },
  { artist: '容祖儿', song: '怯' },
  { artist: '安全着陆', song: '个人简介' },
  { artist: '宋岳庭', song: '为何我' },
  { artist: '官恩娜', song: '暗恋航空' },
  { artist: '小乐哥（王唯乐）', song: '慢慢' },
  { artist: '小乐哥（王唯乐）', song: '执迷不悟 (DJ版)' },
  { artist: '小乐哥（王唯乐）', song: '执迷不悟' },
  { artist: '容祖儿', song: '想得太远' },
  { artist: '容祖儿', song: '抱抱' },
  { artist: '容祖儿', song: '早有预谋' },
  { artist: '徐梦圆', song: 'China-A' },
  { artist: '徐梦圆', song: 'China-C' },
  { artist: '徐梦圆', song: 'China-E' },
  { artist: '影山浩宣', song: 'CHA-LA HEAD-CHA-LA' },
  { artist: '徐佳莹', song: '一样的月光' },
  { artist: '徐俊雅', song: '爱你（把我们衣服纽扣互扣）' },
  { artist: '徐梦圆', song: 'China-Y' },
  { artist: '徐梦圆', song: 'color-X' },
  { artist: '徐梦圆', song: 'ninelie (Easylistening)' },
  { artist: '徐梦圆', song: 'China-G' },
  { artist: '徐梦圆', song: 'China-O' },
  { artist: '徐梦圆', song: 'China-X' },
  { artist: '张学友', song: '吻别' },
  { artist: '张学友', song: '饿狼传说' },
  { artist: '张惠妹', song: '原来你什么都不要' },
  { artist: '广东雨神', song: '广东爱情故事' },
  { artist: '张叶蕾', song: '还是分开' },
  { artist: '张国荣', song: 'Monica' },
  { artist: '张芸京', song: '偏爱' },
  { artist: '张韶涵', song: '淋雨一直走' },
  { artist: '当山ひとみ', song: 'CATHY (2021 Remaster)' },
  { artist: '张惠妹', song: '我可以抱你吗' },
  { artist: '张昊晴', song: '最后一页' },
  { artist: '张紫豪', song: '可不可以' },
  { artist: 'Martin Garrix _ David Guetta _ Jamie Scott _ Romy Dya', song: 'So Far Away' },
  { artist: 'Vicetone', song: 'Tremble' },
  { artist: 'Vienna Radio Symphony Orchestra', song: 'Une petite musique de nuit in G Major (G大调弦乐小夜曲)' },
  { artist: 'Village People', song: 'Y_M_C_A_' },
  { artist: 'Vexento', song: 'Lonely Dance' },
  { artist: 'Vexento', song: 'Trippy Love' },
  { artist: 'Vibebox', song: 'Mii Channel Theme' },
  { artist: 'WILLOW', song: 'Wait a Minute!' },
  { artist: 'Worry Free Studios', song: 'Diamond Cut Diamond (Original Mix)' },
  { artist: 'XloveZ_徐梦圆', song: 'ninelie(Easylistening) (Inst_)' },
  { artist: 'Vivienne', song: 'Embrace' },
  { artist: 'Voxlight', song: 'Lolita' },
  { artist: 'Wafia_Louis the child', song: 'Better Not' },
  { artist: 'Toby Fox', song: 'sans_' },
  { artist: 'Toby Fox', song: 'Shop' },
  { artist: 'Toni Braxton', song: 'Sposed To Be' },
  { artist: 'Toby Fox', song: 'Death By Glamour' },
  { artist: 'Toby Fox', song: 'His Theme' },
  { artist: 'Toby Fox', song: 'MEGALOVANIA' },
  { artist: 'Unlike Pluto', song: 'Wannabe' },
  { artist: 'USAO _ Shandy kubota', song: 'Accents' },
  { artist: 'Various Artists', song: 'Watch Out (Main Track_Full Length)' },
  { artist: 'TRUE (唐沢美帆)', song: 'Sincerely' },
  { artist: 'Tsundere Twintails', song: 'Hold Music' },
  { artist: 'UMI', song: 'Down to Earth' },
  { artist: 'そらる', song: '天ノ弱' },
  { artist: 'ちょろゴンず', song: 'イシュカン・コミュニケーション (异种族间的交流)' },
  { artist: 'でんの子P', song: 'もうどくちゅうい (猛毒注意)' },
  { artist: 'ずっと真夜中でいいのに。 (永远是深夜有多好｡)', song: '勘冴えて悔しいわ (直觉敏锐而不甘)' },
  { artist: 'ずっと真夜中でいいのに。', song: 'MILABO' },
  { artist: 'ずっと真夜中でいいのに。', song: 'クリームで会いにいけますか' },
  { artist: 'ダイヤモンドダスト', song: 'ETERNAL FLAME ~空の箱~ (ETERNAL FLAME ~空箱~)' },
  { artist: 'トゲナシトゲアリ', song: '空の箱(Vo_河原木桃香)' },
  { artist: '七草くりむ', song: 'だいあるのーと (Dial Note)' },
  { artist: 'にゃーろんず', song: 'Apple Pie' },
  { artist: 'アトラスサウンドチーム', song: 'Suspicious Person' },
  { artist: 'カニ研究会', song: '我愛你-上海蟹-' },
  { artist: 'Zachz Winner', song: 'Love For You' },
  { artist: 'Zerky _ Liu', song: 'Temple' },
  { artist: 'α·Pav', song: 'Eta' },
  { artist: 'Xyisx', song: '风驶过的声音是 (Remix_Slowed+Reverd)' },
  { artist: 'yama', song: '春を告げる (宣告春天)' },
  { artist: 'Yasper', song: 'Good Friends (Original Mix)' },
  { artist: 'さユり (Sayuri)', song: '光と闇 (光与暗) (弾き語りver_)' },
  { artist: 'しゃろう', song: '3_03 PM' },
  { artist: 'ずっと真夜中でいいのに。 (永远是深夜有多好｡)', song: 'ミラーチューン (Mirror Tune)' },
  { artist: 'Τ Λ Я Λ', song: 'Plastic Love (TARA Bootleg)' },
  { artist: 'Молчат Дома', song: 'Клетка' },
  { artist: 'さユり (Sayuri)', song: 'フラレガイガール (被甩男女)' },
  { artist: 'Re_Plus _ 二宮愛', song: 'Goodbye to Your Love' },
  { artist: 'Richard Clayderman', song: 'Matrimonio De Amor (梦中的婚礼) (Inst_)' },
  { artist: 'Romy Wave', song: 'Waiting For Love' },
  { artist: 'RADWIMPS', song: '風たちの声 (Movie edit_Inst)' },
  { artist: 'Raujika', song: 'Bamboo' },
  { artist: 'REDTABEATS', song: 'Assumptions' },
  { artist: 's0rrow', song: 'unhappy' },
  { artist: 'sammy', song: '三葉のテーマ (Piano Cover)' },
  { artist: 'Sasunive', song: 'Helltaker' },
  { artist: 'ROSE BEAT', song: 'Life Goes On (Remix)' },
  { artist: 'ROUND TABLE (ラウンド・テーブル)_にの', song: 'Rainbow' },
  { artist: 'Ryan', song: '冰镇冷萃' },
  { artist: 'Palpit', song: 'My Happy Song (Remix)' },
  { artist: 'park bird', song: 'Are You Lost' },
  { artist: 'Patrice Rushen', song: 'Forget Me Nots (Remastered)' },
  { artist: 'Odjbox', song: 'Otto Croy' },
  { artist: 'Oh The Larceny', song: 'This Is It' },
  { artist: 'Owl City_Carly Rae Jepsen', song: 'Good Time' },
  { artist: 'Queen', song: 'Bohemian Rhapsody' },
  { artist: 'Queen', song: 'Love Of My Life (2011 remastered)' },
  { artist: 'RADWIMPS', song: '大丈夫 (Movie edit_Inst)' },
  { artist: 'Patrice Rushen', song: 'Forget Me Nots' },
  { artist: 'P丸様｡', song: 'ないばいたりてぃ (不分青红皂白)' },
  { artist: 'Quadro Nuevo', song: 'Tu Vuo Fa LAmericano' },
  { artist: 'The 1999_Alex Sandra (Aleksandra Marshaniia)', song: '一张唱片的使命 A Records Odyssey' },
  { artist: 'The Beatles', song: 'Hey Jude' },
  { artist: 'The Beatles', song: 'Let It Be' },
  { artist: 'TAIGA', song: 'Middle Of The Night (Original Mix)' },
  { artist: 'Teenear', song: 'Come See Me' },
  { artist: 'Teyana Taylor', song: 'Bare Wit Me' },
  { artist: 'TheFatRat', song: 'Unity' },
  { artist: 'TheFatRat', song: 'Windfall' },
  { artist: 'Tobu', song: 'Higher' },
  { artist: 'The Miracles', song: 'Come And Get Your Love' },
  { artist: 'The Weeknd', song: 'Blinding Lights' },
  { artist: 'The xx', song: 'Intro' },
  { artist: 'She & Him', song: 'I Thought I Saw Your Face Today' },
  { artist: 'Shenseea', song: 'Deserve It' },
  { artist: 'Skrillex _ Sirah', song: 'Bangarang' },
  { artist: 'Satellite Lovers', song: 'Sunnyday, Holiday' },
  { artist: 'Saykoji', song: 'Rasa Sayange (Remix)' },
  { artist: 'Shawn Wasabi_Mothica_Pusher', song: 'Clear (Shawn Wasabi Remix)' },
  { artist: 'SWIN', song: 'S - 只因你太美' },
  { artist: 'SynBlazer', song: '10%' },
  { artist: 'T', song: 'Pain_Ne-Yo - Turn All the Lights On' },
  { artist: 'Slipfunc', song: 'Glance' },
  { artist: 'Storm Lake', song: 'Forget (Slowed Down)' },
  { artist: 'supercell_初音未来 (初音ミク)', song: 'ワールドイズマイン (World is Mine)' },
  { artist: '三轮学 _ MANYO', song: 'a memories for us feat_＂Days＂' },
  { artist: '曲婉婷', song: 'Jar Of Love (Album Version)' },
  { artist: '曲婉婷', song: '我的歌声里' },
  { artist: '朴志胤', song: '성인식(Coming of age ceremony) (成人礼)' },
  { artist: '方大同', song: 'Baby girl (Demo)' },
  { artist: '日向文', song: '落下 (いっぱつにゅうこんver_)' },
  { artist: '暮肆_Mousse', song: '2009-2020动漫单曲' },
  { artist: '李荣浩', song: '年少有为' },
  { artist: '李荣浩', song: '我看着你的时候' },
  { artist: '李荣浩', song: '戒烟' },
  { artist: '李孝利 (이효리)', song: '10 MINUTES' },
  { artist: '李荣浩', song: '不遗憾' },
  { artist: '李荣浩', song: '喜剧之王' },
  { artist: '幻想动漫音乐', song: '告别' },
  { artist: '幻想动漫音乐', song: '妖灵会馆' },
  { artist: '广桥真纪子', song: 'いのちの名前 (ピアノ)' },
  { artist: '市川淳 (JUN ICHIKAWA)', song: '-祈り- (祈祷) (ヨスガノソラ メインテーマ)' },
  { artist: '市川淳', song: 'ハルカノココロ (遥远的心)' },
  { artist: '平野绫 (ひらの あや)', song: 'God knows___' },
  { artist: '打工战士', song: '每天一遍,可莉完蛋' },
  { artist: '打工战士', song: '派蒙就是进不去' },
  { artist: '放課後ティータイム (放学后TEA TIME)', song: 'Don’t say “lazy” (5人Ver_)' },
  { artist: '张紫宇', song: 'Track In Time' },
  { artist: '徐梦圆', song: 'Color-X 3D' },
  { artist: '戴荃', song: '悟空' },
  { artist: '松下優也 (まつした ゆうや)', song: 'Mr_Broken Heart (心碎先生)' },
  { artist: '松田彬人', song: 'Welcome to the Chu-2 byo World!' },
  { artist: '松田彬人', song: '浮かれ笑顔でGO！ (带着愉快的笑颜GO)' },
  { artist: '来一碗老于', song: '赤伶 (21秒片段)' },
  { artist: '来一碗老于', song: '风吹丹顶鹤 (Live)' },
  { artist: '杰不懂浪漫', song: '你听得到 (31秒片段)' },
  { artist: '梁咏琪', song: '灰姑娘' },
  { artist: '洛天依', song: '洛天依投食歌' },
  { artist: '洛天依', song: '非人哉' },
  { artist: '柳轻颂', song: 'Lot to Learn' },
  { artist: '柿原朱美', song: 'Blue Apple (蓝苹果)' },
  { artist: '桃音モモ', song: 'Nyan Cat (彩虹猫)' },
  { artist: '李荣浩 _ 王者荣耀', song: '后羿' },
  { artist: '来一碗老于', song: '侧脸 (片段版)' },
  { artist: '来一碗老于', song: '兰亭序 (19秒片段)' },
  { artist: '李荣浩', song: '满座' },
  { artist: '李荣浩', song: '爸爸妈妈' },
  { artist: '李荣浩', song: '落俗' },
  { artist: '来一碗老于', song: '把回忆拼好给你 (Live)' },
  { artist: '来一碗老于', song: '牵丝戏' },
  { artist: '来一碗老于', song: '玫瑰花的葬礼 (Live)' },
  { artist: '来一碗老于', song: '剑魂' },
  { artist: '来一碗老于', song: '反方向的钟' },
  { artist: '来一碗老于', song: '小情歌 (Live)' },
  { artist: '具島直子', song: 'no_ no_ no_' },
  { artist: '冈崎律子', song: 'For フルーツバスケット (For Fruits Basket)' },
  { artist: '冈崎律子', song: 'For フルーツバスケット (水果篮子)' },
  { artist: '余佳运', song: '和你' },
  { artist: '具島直子', song: '9月の海' },
  { artist: '具島直子', song: 'Candy' },
  { artist: '初音未来 (初音ミク)', song: 'ねこみみスイッチ (猫耳开关)' },
  { artist: '初音未来 (初音ミク)', song: 'ぽっぴっぽー (蔬菜汁)' },
  { artist: '初音未来 (初音ミク)', song: '千本桜' },
  { artist: '刘俊骐', song: '好喜欢你' },
  { artist: '刘英杰', song: 'Magnolia' },
  { artist: '初音ミク_ピノキオピー', song: 'loveit (爱它)' },
  { artist: '不会走调的琴', song: '温舒娴 (美梦0_8)' },
  { artist: '东百下小雨', song: '回 忆 凌 迟' },
  { artist: '中国交响乐团', song: '拉德斯基进行曲 (Radetzky March)' },
  { artist: '上海乐团管弦乐队', song: 'Dagger Society Suite_ Overture_ Daggers Society Overture' },
  { artist: '上海民族乐团', song: '喜洋洋' },
  { artist: '上野大典', song: 'プーチンのテーマ (SPECIAL REMIX)' },
  { artist: '买辣椒也用券', song: '起风了' },
  { artist: '于果', song: '侧脸' },
  { artist: '井鈣', song: 'GBC日常小曲' },
  { artist: '中国交响乐团', song: '维瓦尔第：四季·春 - 第一乐章 快板' },
  { artist: '中国人民解放军军乐团', song: '步步高' },
  { artist: '中央民族乐团', song: '梁祝' },
  { artist: '孙燕姿', song: '我怀念的' },
  { artist: '安瀬聖', song: 'アマオト' },
  { artist: '宋冬野', song: '安和桥' },
  { artist: '塞壬唱片', song: 'MSR _ 二宮愛 _ DJ OKAWARI - Speed of Light' },
  { artist: '天狗食兔', song: '新宝岛 (44秒Live片段)' },
  { artist: '孙燕姿', song: '我不难过' },
  { artist: '小乐哥（王唯乐）', song: '伯虎说' },
  { artist: '小黑屋バカ', song: '锤子之歌' },
  { artist: '岩崎琢 (いわさき たく)', song: 'Awake' },
  { artist: '宋冬野', song: '斑马，斑马' },
  { artist: '宋冬野', song: '董小姐' },
  { artist: '官恩娜', song: '戏中有气' },
  { artist: '华晨宇', song: '智商二五零 (Live)' },
  { artist: '南拳妈妈', song: 'Here We Go' },
  { artist: '南拳妈妈', song: '下雨天' },
  { artist: '初音未来 (初音ミク)', song: '深海少女' },
  { artist: '初音未来 (初音ミク)_すこっぷ (scop)', song: 'アイロニ (反语)' },
  { artist: '加藤英美里', song: '100%_ナイナイナイ (100％_没有没有没有) (100％_没有没有没有) (Single Version)' },
  { artist: '塞壬唱片', song: 'MSR _ David Westbom - 多索雷斯假日' },
  { artist: '塞壬唱片', song: 'MSR _ DJ OKAWARI _ 澤田かおり - Everythings Alright' },
  { artist: '塞壬唱片', song: 'MSR _ Elvin Shen _ ZT _ Erik Castro _ David Lin _ 左乙 - Misty Memory (Night Version)' },
  { artist: '南辞', song: '干嘛听苦情歌' },
  { artist: '双笙 (陈元汐)', song: '心做し (心理作用)' },
  { artist: '周深', song: 'Rubia' },
  { artist: 'Elephante_Matluck', song: 'Come Back For You' },
  { artist: 'Elisa', song: 'Eppure Sentire (un senso di te)' },
  { artist: 'Ella Mai', song: 'Trying' },
  { artist: 'Dogena', song: 'Time' },
  { artist: 'Doja Cat _ SZA', song: 'Kiss Me More' },
  { artist: 'dvsn _ Snoh Aalegra', song: 'Between Us (feat_ Snoh Aalegra) (Clean)' },
  { artist: 'Evan Call', song: 'Another Sunny Day (另一个晴天)' },
  { artist: 'Evan Call', song: 'Hymn to the Sea Pt_ 1' },
  { artist: 'Evan Call', song: 'Never Coming Back' },
  { artist: 'Eminem', song: 'Stan' },
  { artist: 'EmoCosine', song: 'MAGENTA POTION (Extended Mix)' },
  { artist: 'Euge Groove', song: 'Its Only Rain' },
  { artist: 'Deep Chills_IVIE', song: 'Run Free (Original Mix)' },
  { artist: 'Dennis Kuo', song: 'A Broken Heart Heals with Time' },
  { artist: 'Dennis Kuo', song: 'Track in Time' },
  { artist: 'DATEKEN', song: 'ワンルーム・オール・ザット・ジャズ (one room, all that jazz !)' },
  { artist: 'Daydreaming', song: '焦糖玛奇朵' },
  { artist: 'DECO_27 (デコ・ニーナ)_初音未来 (初音ミク)', song: 'ラビットホール (兔子洞)' },
  { artist: 'DJ Mexx_DJ Dikson', song: '#ТАМАДА (DJ Mexx & DJ Dikson Radio Remix)' },
  { artist: 'Django Reinhardt', song: 'Novel Pets' },
  { artist: 'DM DOKURO', song: 'The Tale of a Cruel World' },
  { artist: 'Dion Timmer _ The Arcturians', song: 'The Best Of Me' },
  { artist: 'Dj Blyatman_Russian Village Boys', song: 'Cyka Blyat' },
  { artist: 'Dj Electronic', song: 'Hymn For The Weekend (Remix)' },
  { artist: 'Guillaume Denis_Juan', song: 'Carlo Manni - Edamame' },
  { artist: 'György Széll _ The Cleveland Orchestra', song: '《费加罗的婚礼》序曲' },
  { artist: 'G_E_M_ 邓紫棋', song: '龙卷风' },
  { artist: 'Git Fresh', song: 'Booty Music' },
  { artist: 'glaive', song: 'the prom' },
  { artist: 'GRABOTE', song: 'Dancer In The Dark (Original Mix)' },
  { artist: 'HOME MADE 家族 (ホームメードかぞく)', song: 'サンキュー!! (谢谢！！)' },
  { artist: 'HoneyWorks_鎖那 (さな)', song: '可愛くなりたい (feat_ 鎖那) (想变得可爱)' },
  { artist: 'HOYO', song: 'MiX - 太空漫步 Space Walk' },
  { artist: 'h6itam _ n7san7os _ MC Menor do Alvorada', song: 'MONTAGEM ALQUIMIA' },
  { artist: 'Henry Young_Ashley Alisha', song: 'One More Last Time' },
  { artist: 'Herbert von Karajan', song: 'An die schonen blauen Donau, Op_ 314 (蓝色多瑙河) (1987 - Remaster)' },
  { artist: 'Feint', song: 'My Sunset (Original Mix)' },
  { artist: 'Fine乐团', song: '呼吸决定' },
  { artist: 'Frank Ocean', song: 'White Ferrari' },
  { artist: 'Evan Call', song: 'One Last Message' },
  { artist: 'Evan Call', song: 'The Voice in My Heart' },
  { artist: 'Everey', song: '炎热星河 (Remix)' },
  { artist: 'Fusq _ Mylk', song: 'Blush (feat_ Mylk)' },
  { artist: 'Georg Solti', song: 'Messiah - Highlights - Halleluljah (弥赛亚 - 精选集 - 哈利路亚)' },
  { artist: 'GET IN THE RING (みぃ)', song: 'Thank you for dears_' },
  { artist: 'Franz Liszt Chamber Orchestra_Janos Rolla', song: 'String Serenade No_ 13 in G Major, K_ 525, _Eine kleine Nachtmusik_ - I_ Allegro (G大调第13号弦乐小夜曲，作品525 “小夜曲” - 第一乐章 快板)' },
  { artist: 'From Somewhere Quiet', song: 'Cambridge' },
  { artist: 'Funkyyy', song: '!!!PHONK!!!' },
  { artist: 'Arthur Grumiaux', song: 'Mozart - Divertimento in D, K_334 - Menuet' },
  { artist: 'Arthur Grumiaux _ Istvan Hajdu', song: 'Humoresque - G-Flat Major (滑稽曲_幽默曲_诙谐曲)' },
  { artist: 'Ash', song: 'Melody (Original Mix)' },
  { artist: 'ancient heiakim', song: 'ehe te nandayo pt2' },
  { artist: 'Andrei Machado', song: 'Sopro seu' },
  { artist: 'André Rieu _ Johann Strauss Orchestra', song: 'Libiamo (From _La Traviata_) (Drinking Song)' },
  { artist: 'Ayumi_', song: 'fil___' },
  { artist: 'Befour', song: 'Magic Melody' },
  { artist: 'Bishop Briggs', song: 'River' },
  { artist: 'Austin Farwell', song: 'Sweet Dreams' },
  { artist: 'Austin Farwell', song: 'Waltz of the Wind' },
  { artist: 'Avicii_Aloe Blacc', song: 'SOS' },
  { artist: '88century', song: 'petit poney' },
  { artist: '88rising_Rich Brian', song: 'History' },
  { artist: 'Abilene', song: 'Ola' },
  { artist: '2 Chainz_Jason Derulo', song: 'Talk Dirty' },
  { artist: '3rd Coast', song: 'My Jealousy (Original ver)' },
  { artist: '4円', song: 'アイロニ (反语)' },
  { artist: 'Agrupacion LatinHits', song: 'No Sabes Cuanto Te Quiero' },
  { artist: 'Alessia Cara、Lucian', song: 'Here (Lucian Remix)' },
  { artist: 'Amerie', song: '1 Thing' },
  { artist: 'Adele', song: 'Rolling in the Deep' },
  { artist: 'ADTurnUp', song: 'Late Night Drive' },
  { artist: 'Aevv', song: 'Love Philter' },
  { artist: 'conner wells', song: 'Off My Mind' },
  { artist: 'Corn Wave', song: 'evening' },
  { artist: 'Corn Wave', song: 'манго нектар' },
  { artist: 'Cloke _ Shiloh Dynasty', song: 'Novocaine 2' },
  { artist: 'Club Unicorn', song: 'Ylang Ylang' },
  { artist: 'cm', song: 'letter' },
  { artist: 'culicc', song: '溶けた声で' },
  { artist: 'cyberlume', song: 'One More Day (Can You)' },
  { artist: 'Daoko (ダヲコ)_米津玄師 (よねづ けんし)', song: '打上花火' },
  { artist: 'COSMIC CYCLER', song: 'Pure Love' },
  { artist: 'Craig David', song: 'Insomnia' },
  { artist: 'Crystal Castles', song: 'Kerosene' },
  { artist: 'Breakbot_Irfane', song: 'Baby Im Yours (feat_ Irfane)' },
  { artist: 'Brooks_GRX', song: 'Boomerang' },
  { artist: 'Budapest Strings _ Bela Banfalvi', song: 'String Quintet in E Major, Op_ 11, No_ 5, G_ 275_ III_ Minuetto (arr_ for orchestra)_ String Quintet in E Major, Op_ 11, No_ 5, G_ 275_ III_ Minuetto' },
  { artist: 'Blanks', song: 'Better Now' },
  { artist: 'BLU', song: 'SWING (ブルー・スウィング) - Sunset (2019 Y_Nakamura Remastering)' },
  { artist: 'Brandy', song: 'I Wanna Be Down (2F Remix)' },
  { artist: 'cinnamons_evening cinema', song: 'summertime (夏日时光)' },
  { artist: 'Classical Artists', song: '巴赫：第一号无伴奏大提琴组曲前奏曲《寻羊冒险记》 (Inst_)' },
  { artist: 'Cloke', song: 'Novocaine 2' },
  { artist: 'C418', song: 'Minecraft' },
  { artist: 'Christian Kuria', song: 'Losing You' },
  { artist: 'CHROMANCE', song: 'Wrap Me In Plastic' },
  { artist: 'Hoàng Thùy Linh', song: 'See Tình - Cucak Remix' },
  { artist: 'm', song: 'flo - prism (Original)' },
  { artist: 'M3mo', song: '假心话' },
  { artist: 'mabanua (マバヌア)', song: 'The theme of Sachio' },
  { artist: 'Luís Alvarez', song: 'Hikari' },
  { artist: 'Låpsley', song: 'Hurt Me' },
  { artist: 'm', song: 'flo (陨-浮流) - come again___ and AGAIN!' },
  { artist: 'Manafest', song: 'Edge of My Life' },
  { artist: 'Maroon 5', song: 'One More Night (Sticky K Remix)' },
  { artist: 'Maroon 5_J Balvin', song: 'Maps (Rumba Whoa Remix)' },
  { artist: 'Mac Ayres', song: 'slooow down' },
  { artist: 'mamatjon', song: 'PYRO (remix_ Chester Young_Castion_Remix)' },
  { artist: 'mamerico (マメリコ)', song: 'my room' },
  { artist: 'Lil Liquorice_Съездбек Искеналиев_Coems', song: 'Coems' },
  { artist: 'Liyuu', song: 'Oh! レディ・ステディ・ポジティブ (哦！就绪·稳定·积极)' },
  { artist: 'LLoyd', song: 'Lay It Down' },
  { artist: 'Leateq', song: 'Tokyo (Sped Up)' },
  { artist: 'Ledisi', song: 'I Blame You' },
  { artist: 'Lianne La Havas', song: 'Cant Fight' },
  { artist: 'LORDSUN', song: 'あなたは知らMAGIC CITY (你知道MAGIC CITY) (你知道MAGIC CITY)' },
  { artist: 'Loud Luxury', song: 'See It Again' },
  { artist: 'Ludovico Einaudi', song: 'Einaudi - Nuvole Bianche (鲁多维科・艾奥迪：白云)' },
  { artist: 'lodoni', song: 'Say Goodbye' },
  { artist: 'Lofi Multiverse_Slowedverse', song: 'Runaway (feat_ Slowedverse)' },
  { artist: 'London Music Works', song: 'Frolic (From _Curb Your Enthusiasm_)' },
  { artist: 'Mustard_Roddy Ricch', song: 'Ballin' },
  { artist: 'MyGO!!!!!', song: '回層浮' },
  { artist: 'MyGO!!!!!', song: '孤壊牢' },
  { artist: 'Monogem', song: 'Wild' },
  { artist: 'Muni Long', song: 'Sneaky Link' },
  { artist: 'Musicbot', song: '珠玉 (AI 方大同)' },
  { artist: 'naotyu', song: '- 動悸 (悸动)' },
  { artist: 'Ni_Co', song: 'Bleeding Love' },
  { artist: 'nyamura', song: '超デジタル超デトックス' },
  { artist: 'MyGO!!!!!', song: '春日影 (MyGO!!!!! ver_)' },
  { artist: 'MyGO!!!!!', song: '端程山' },
  { artist: 'MyGO!!!!!', song: '輪符雨' },
  { artist: 'Martin Jensen_RANI', song: 'At Least I Had Fun' },
  { artist: 'MEGA', song: '東 京 尋 情 記' },
  { artist: 'Michael Giacchino', song: 'Light the Fuse' },
  { artist: 'Marshmello', song: 'Flash Funk (Marshmello Remix)' },
  { artist: 'Martin I Oden', song: 'So Cute' },
  { artist: 'Martin Jensen', song: 'Solo Dance' },
  { artist: 'miu', song: 'clips (阿藤芳史) - 絆 (羁绊)' },
  { artist: 'mixed matches _ Lukrative', song: 'Maybe' },
  { artist: 'MK', song: '17 (In the Air Dub)' },
  { artist: 'Michael Giacchino', song: 'Married life' },
  { artist: 'Michael Jackson', song: 'Rock With You' },
  { artist: 'Mitsukiyo', song: 'Mischievous Step' },
  { artist: 'Jonas Blue_RAYE', song: 'By Your Side' },
  { artist: 'July', song: 'Revert (返回去)' },
  { artist: 'Justin Timberlake', song: 'SexyBack' },
  { artist: 'Jenevieve', song: 'Baby Powder' },
  { artist: 'Johann Strauss Jr_', song: '春之声圆舞曲 (Frühlingsstimmen,Walzer, Op_410)' },
  { artist: 'John Williams _ London Symphony Orchestra', song: 'Cantina Band (威廉斯：酒吧乐团曲)' },
  { artist: 'János Starker', song: 'Suite for Cello Solo No_ 1 in G Major, BWV_ 1007 - I_ Prélude (G大调第1号大提琴组曲，作品1007 - 第1首：前奏曲 - G大调第1号大提琴独奏组曲，作品1007 ， 第一乐章 前奏曲)' },
  { artist: 'KALEO', song: 'Way down We Go' },
  { artist: 'Kana', song: 'ハンマーを電波ソングにしてみた (锤子电波歌)' },
  { artist: 'JVKE', song: 'golden hour' },
  { artist: 'JVKE', song: 'this is what sadness feels like (间奏版)' },
  { artist: 'JVKE', song: 'this is what winter feels like' },
  { artist: 'Ivoris', song: 'I Wish My Mind Would Shut Up' },
  { artist: 'Ivoris', song: 'kiss me more' },
  { artist: 'iwamizu', song: 'Love at First Sight' },
  { artist: 'ilyeucli _ 初音未来', song: 'Miku Turned into a Cat! (feat_ Hatsune Miku)' },
  { artist: 'Imagine Dragons_英雄联盟', song: 'Enemy (from the series Arcane League of Legends)' },
  { artist: 'Interupt _ Luna LePage', song: 'Power (In Your Soul)' },
  { artist: 'Jamie Berry_Octavia Rose', song: 'Lost In The Rhythm (Original Mix)' },
  { artist: 'jaycd', song: '未闻花名 (remix_ 茅野愛衣_钢琴曲纯音乐)' },
  { artist: 'Jeff李', song: '杜鹃圆舞曲' },
  { artist: 'iwamizu', song: 'Rinki' },
  { artist: 'Jacob LaVallee', song: 'Respite' },
  { artist: 'James Carter_Nevve', song: 'Hands in the Fire' },
  { artist: 'King CAAN_ELYSA', song: 'Go Again (feat_ ELYSA)' },
  { artist: 'Kira_辣椒酱', song: '绊' },
  { artist: 'KittenfanVR', song: 'The Snare (Clap!)' },
  { artist: 'Kiana Ledé', song: 'Feel A Way_' },
  { artist: 'Kiana Ledé', song: 'Honest_' },
  { artist: 'Kiana Ledé', song: 'Title' },
  { artist: 'Laura Shigihara', song: 'Zombies on Your Lawn' },
  { artist: 'Lay Lady Lay', song: 'RIFF' },
  { artist: 'Leandro Aconcha', song: 'The Entertainer' },
  { artist: 'ko', song: 'ko-ya - 小鳥のワルツ' },
  { artist: 'Kurousa', song: 'P (黒うさP)_初音未来 (初音ミク) - 千本桜 (Game Version)' },
  { artist: 'Lara梁心颐', song: 'Gentleman' },
  { artist: 'Kevin MacLeod', song: 'Breaktime' },
  { artist: 'Kevin MacLeod', song: 'Carefree' },
  { artist: 'Kevin MacLeod', song: 'Hyperfun' },
  { artist: 'Kelly Rowland', song: 'Love You More At Christmas Time' },
  { artist: 'Kelly Rowland _ Lord Quest', song: 'Dont You Worry' },
  { artist: 'kenzie', song: 'Breathe' },
  { artist: 'Kevin MacLeod', song: 'Olde Timey' },
  { artist: 'Kevin MacLeod', song: 'Pixel Peeker Polka - faster' },
  { artist: 'Kiana Ledé', song: 'EX' },
  { artist: 'Kevin MacLeod', song: 'If I Had a Chicken' },
  { artist: 'Kevin MacLeod', song: 'Local Forecast - Elevator' },
  { artist: 'Kevin MacLeod', song: 'Merry Go' },
  { artist: '浅野隼人', song: '小手鞠' },
  { artist: 'Hawk Nelson_Jonathan Steingard', song: 'Sold Out' },
  { artist: 'Hayden James_Boy Matthews', song: 'Just Friends' },
  { artist: 'Hedley', song: 'Lose Control' },
  { artist: 'G_E_M_ 邓紫棋', song: '画 (Live Piano Session Ⅱ)' },
  { artist: 'Hampton the Hampster', song: 'The HampsterDance Song' },
  { artist: 'Hard working jack', song: 'What Are Words' },
  { artist: 'Hoàng Thùy Linh', song: 'See Tình (Speed Up Version)' },
  { artist: 'Humbert Humbert', song: 'しろつめくさ (白诘草)' },
  { artist: 'H_E_R_ _ Chris Brown', song: 'Come Through' },
  { artist: 'Hidetake Takayama', song: 'PUKE' },
  { artist: 'Hillsong Young & Free', song: 'Wake (Live)' },
  { artist: 'Hillsong Young & Free', song: 'Wake (Studio)' },
  { artist: 'Gideon Matthew', song: 'Gentle Embrace' },
  { artist: 'Gotye_Kimbra', song: 'Somebody That I Used to Know' },
  { artist: 'Grover Washington Jr_ _ Bill Withers', song: 'Just the Two of Us (Edit)' },
  { artist: 'F_O_O_L', song: 'Criminals' },
  { artist: 'GALA', song: '追梦赤子心' },
  { artist: 'Gesaffelstein', song: 'Aleph' },
  { artist: 'G_E_M_ 邓紫棋', song: '唯一' },
  { artist: 'G_E_M_ 邓紫棋', song: '来自天堂的魔鬼' },
  { artist: 'G_E_M_ 邓紫棋', song: '泡沫' },
  { artist: 'Gwen Stefani', song: 'Luxurious (Album Version)' },
  { artist: 'G_E_M_ 邓紫棋', song: '倒数' },
  { artist: 'G_E_M_ 邓紫棋', song: '光年之外' },
  { artist: 'Jai Wolf', song: 'Indian Summer' },
  { artist: 'Jake Miller', song: 'Rumors' },
  { artist: 'Jamelia', song: 'Superstar' },
  { artist: 'IN', song: 'K_王忻辰 - 回忆观影券 (伴奏)' },
  { artist: 'J Lisk', song: 'Where Is Your Love' },
  { artist: 'Jagged Edge _ Ashanti', song: 'Put A Little Umph In It (Album Version)' },
  { artist: 'Jason Derulo_2 Chainz', song: 'Talk Dirty' },
  { artist: 'Jaymes Young', song: 'Infinity' },
  { artist: 'Jenevieve', song: 'Résumé' },
  { artist: 'James Blunt', song: 'Youre Beautiful' },
  { artist: 'James Horner', song: 'The Ludlows (From _Legends Of The Fall_ Soundtrack) (勒德洛一家)' },
  { artist: 'James Major', song: 'Natural' },
  { artist: 'iKON', song: 'LOVE SCENARIO (曾经爱过)' },
  { artist: 'ILLENIUM_Phoebe Ryan', song: 'Mine (Illenium Remix)' },
  { artist: 'Imagine Dragons', song: 'Bones' },
  { artist: 'H_E_R_ _ Daniel Caesar', song: 'Best Part' },
  { artist: 'Ice Paper', song: '中间人' },
  { artist: 'Ice Paper', song: '心如止水' },
  { artist: 'imase', song: 'NIGHT DANCER' },
  { artist: 'imase', song: '逃避行' },
  { artist: 'IN', song: 'K _ 安苏羽 _ 傅梦彤 - 潮汐(Natural)' },
  { artist: 'Imagine Dragons_Broiler', song: 'Shots (Broiler Remix)' },
  { artist: 'Imagine Dragons_JID_英雄联盟', song: 'ENEMY (宿敌)' },
  { artist: 'Imanbek_BYOR', song: 'Belly Dancer' },
  { artist: 'Edvard Grieg_Artur Rodzinski_Philharmonic Symphony Orchestra of London', song: 'Peer Gynt Suite No_ 1, Op_ 46 - I_ Morning (培尔金特组曲第1号，作品46 - 第1首 晨歌)' },
  { artist: 'EGOIST', song: 'Departures 〜あなたにおくるアイの歌〜 (离别 ~赠予你的爱之歌~)' },
  { artist: 'EGOIST', song: 'この世界で見つけたもの (在这个世界找寻到的东西)' },
  { artist: 'Eagles', song: 'Hotel California' },
  { artist: 'Earth, Wind & Fire', song: 'September' },
  { artist: 'Ed Sheeran', song: 'Shape of You' },
  { artist: 'Ellie Goulding', song: 'Still Falling For You (Laibert Remix)' },
  { artist: 'Elliot Moss', song: 'Slip' },
  { artist: 'ElyOtto', song: 'SugarCrash!' },
  { artist: 'EGOIST', song: 'エウテルペ (欧忒耳佩)' },
  { artist: 'EKKSTACY', song: 'i walk this earth all by myself' },
  { artist: 'Ella Mai _ H_E_R_', song: 'Gut Feeling' },
  { artist: 'DJ_Lee', song: '眼鼻嘴 (DJ版)' },
  { artist: 'dkj', song: 'Aces' },
  { artist: 'Doja Cat', song: 'Roll With Us' },
  { artist: 'DJ OKAWARI_Celeina Ann (セレイナ・アン)', song: 'Addiction' },
  { artist: 'DJ OKAWARI_Celeina Ann (セレイナ・アン)', song: 'Glitter' },
  { artist: 'Dj PINK', song: 'Sanlalala (Remix)' },
  { artist: 'DP龙猪 _ 王云宏 _ 陷阱表哥', song: '翠花' },
  { artist: 'DreamSky', song: '阴天 (间奏节奏版)' },
  { artist: 'Dxrk ダーク', song: 'RAVE' },
  { artist: 'Doja Cat', song: 'Say So' },
  { artist: 'Doja Cat _ The Weeknd', song: 'You Right' },
  { artist: 'Dorian Marko', song: 'Cornfield Chase' },
  { artist: 'fhána', song: '愛のシュプリーム！ (至高无上的爱)' },
  { artist: 'Fifth Harmony_Kid Ink', song: 'Worth It' },
  { artist: 'FIFTY FIFTY (피프티피프티)', song: 'Cupid (丘比特)' },
  { artist: 'Exsire', song: 'YOU BALLIN IN THE WRONG NEIGHBORHOOD (EXTENDED_Explicit)' },
  { artist: 'FADI_KLINKO', song: 'Hold On' },
  { artist: 'Fall Out Boy', song: 'My Songs Know What You Did in the Dark (Light Em Up)' },
  { artist: 'Flo Rida_T', song: 'Pain - Low (Album Version)' },
  { artist: 'Fousheé', song: 'single af' },
  { artist: 'FrankJavCee', song: 'SimpsonWave1995' },
  { artist: 'FIFTY FIFTY (피프티피프티)', song: 'Cupid - Twin Ver_ (FIFTY FIFTY) (Sped Up Version)' },
  { artist: 'FKJ_((( O )))', song: 'Ylang Ylang' },
  { artist: 'Flipped', song: 'Dark Blue' },
  { artist: 'EMMA WAHLIN', song: 'Make A Move' },
  { artist: 'EMOWZ', song: 'miss 右 哈尼' },
  { artist: 'en (王翊恩)', song: '云水谣' },
  { artist: 'Em Beihold', song: 'Numb Little Bug' },
  { artist: 'Ember Island _ Matte', song: 'Umbrella (Matte Remix)' },
  { artist: 'Eminem', song: 'Without Me' },
  { artist: 'Erik Satie _ Seid Muhammad Shah', song: 'Gymnopédies - I_ Lent et douloureux' },
  { artist: 'Euge Groove', song: 'Vinyl' },
  { artist: 'Ever So Blue', song: 'Home' },
  { artist: 'Enya', song: 'O Come, O Come, Emmanuel' },
  { artist: 'Enya', song: 'Only Time' },
  { artist: 'Eric Benet _ Faith Evans', song: 'Feel Good' },
  { artist: 'Jeremy Zucker_Bea Miller', song: 'comethru' },
  { artist: 'Lx24', song: 'Мокрые губы (Relanium, Prezzplay, Deen West Radio Remix)' },
  { artist: 'Lyn (稲泉りん)_アトラスサウンドチーム', song: 'Beneath the Mask' },
  { artist: 'Lyn (稲泉りん)_アトラスサウンドチーム', song: 'Life Will Change' },
  { artist: 'Luh Kel', song: 'Pull Up' },
  { artist: 'Luke Pickman', song: 'Mystery of Love' },
  { artist: 'Lx24', song: 'Мокрые губы (Fast Tempo)' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Life Will Change' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Rivers In the Desert' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'The Whims of Fate' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Beneath the Mask -rain-' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Beneath the Mask' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Last Surprise' },
  { artist: 'Lil Nas X_Billy Ray Cyrus', song: 'Old Town Road (feat_ Billy Ray Cyrus) (Remix)' },
  { artist: 'Lil Nas X_Billy Ray Cyrus', song: 'Old Town Road (Remix)' },
  { artist: 'Lil Tjay_6LACK', song: 'Calling My Phone' },
  { artist: 'Lenka', song: 'Trouble Is a Friend' },
  { artist: 'Lenzi_Soriani_Sily', song: 'Poker Face (Extended Mix)' },
  { artist: 'Lightscape', song: 'Collapsing World (Original Mix)' },
  { artist: 'Loud Luxury_Brando', song: 'Body (AIR249)' },
  { artist: 'Lovelyz', song: 'FALLIN′' },
  { artist: 'Ludovico Einaudi', song: 'Experience' },
  { artist: 'Lipps Inc_', song: 'Funkytown (12_ Version)' },
  { artist: 'LiSA (织部里沙)', song: '紅蓮華' },
  { artist: 'LJones', song: 'Soul Below' },
  { artist: 'Mariah Carey', song: 'Underneath The Stars' },
  { artist: 'Mark Ronson_Bruno Mars', song: 'Uptown Funk' },
  { artist: 'Maroon 5', song: 'One More Night' },
  { artist: 'Malcolm Todd', song: 'Sweet Boy' },
  { artist: 'mao (まお)_ひまわりキッズ', song: '夢をかなえてドラえもん (实现梦想的哆啦A梦)' },
  { artist: 'Mariah Carey', song: 'Bye Bye' },
  { artist: 'Maroon 5_SZA', song: 'What Lovers Do' },
  { artist: 'Maroon 5_Wiz Khalifa', song: 'Payphone' },
  { artist: 'Martin Garrix', song: 'Animals (Original Mix)' },
  { artist: 'Maroon 5_Big Sean', song: 'Maps' },
  { artist: 'Maroon 5_Kendrick Lamar', song: 'Dont Wanna Know (Alternate Version)' },
  { artist: 'Maroon 5_Kendrick Lamar', song: 'Dont Wanna Know' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: '星と僕らと (星星与我们)' },
  { artist: 'M2M', song: 'The Day You Went Away' },
  { artist: 'M83', song: 'Midnight City' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Tokyo Daylight' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Wake Up, Get Up, Get Out There -opening movie version-' },
  { artist: 'Lyn _ アトラスサウンドチーム', song: 'Wake Up, Get Up, Get Out There' },
  { artist: 'Magdy Haddad', song: 'Glow Me' },
  { artist: 'Mahalia', song: 'Karma' },
  { artist: 'majiko (まじ娘)', song: 'アイロニ (反语) (カバー)' },
  { artist: 'M83_Mai Lan', song: 'Go!' },
  { artist: 'Madnap _ Pauline Herr', song: 'Slow Down' },
  { artist: 'Mae Stephens', song: 'If We Ever Broke Up' },
  { artist: 'Justin Timberlake_Timbaland', song: 'SexyBack' },
  { artist: 'J_Fla (제이플라)', song: 'Shape Of You' },
  { artist: 'J_Sheon', song: '别问很可怕' },
  { artist: 'July Tun', song: '清新的小女孩' },
  { artist: 'Just Kiddin _ Camden Cox', song: 'Stay The Night' },
  { artist: 'Justin Bieber_Daniel Caesar_GIVĒON', song: 'Peaches' },
  { artist: 'Kar Play', song: 'Shed a Light (Like Instrumental Mix)' },
  { artist: 'Kastra', song: 'Fool For You' },
  { artist: 'Kate Bush', song: 'Running Up That Hill (A Deal With God)' },
  { artist: 'János Starker_Johann Sebastian Bach', song: 'Cello Suite No_ 1 in G Major, BWV 1007 - I_ Prelude (G大调第1号大提琴组曲，作品1007 - G大调第1号大提琴组曲，作品1007 - 第一乐章 前奏曲)' },
  { artist: 'Kan Gao', song: 'Born a Stranger' },
  { artist: 'Kan Gao _ Laura Shigihara', song: 'For River (Piano_Johnnys Version)' },
  { artist: 'Joep Beving', song: 'Etude' },
  { artist: 'Joep Beving', song: 'Saturday Morning (周六早上)' },
  { artist: 'Joep Beving', song: 'Zoetrope (活动幻镜)' },
  { artist: 'Jerry Folk _ ELOQ', song: 'You Know' },
  { artist: 'Jesse Brown', song: 'sleep in sundays' },
  { artist: 'Jhené Aiko _ Swae Lee', song: 'Sativa' },
  { artist: 'Jori King', song: 'Fatal Love' },
  { artist: 'Josh Vietti', song: 'A Thousand Miles' },
  { artist: 'July (줄라이)', song: 'Somewhere' },
  { artist: 'John Williams', song: 'The British Grenadiers' },
  { artist: 'Jon Lajoie', song: 'Everyday Normal Guy 2' },
  { artist: 'Jonasu', song: 'Black Magic' },
  { artist: 'KoruSe', song: 'Two Different Words (Sigle Edit)' },
  { artist: 'Kuraiinu_Lollia', song: 'Galactic Mermaid' },
  { artist: 'Kurupt _ Nate Dogg', song: 'Space Boogie' },
  { artist: 'Kiri T', song: '我可能是回避型' },
  { artist: 'KOKIA (吉田亚纪子)', song: 'ありがとう… (谢谢…)' },
  { artist: 'Kordhell', song: 'Scopin' },
  { artist: 'LBI利比（时柏尘）', song: '小城夏天' },
  { artist: 'LBI利比（时柏尘）', song: '跳楼机' },
  { artist: 'Lenka', song: 'The Show' },
  { artist: 'Kyle Dixon & Michael Stein', song: 'Kids' },
  { artist: 'L Y V E T', song: 'i always sad but my eyes smile' },
  { artist: 'Latto _ Lil Baby', song: 'Sex Lies' },
  { artist: 'Kelly Clarkson', song: 'Catch My Breath' },
  { artist: 'Kelly Rowland _ Beyoncé _ Michelle Williams', song: 'You Changed' },
  { artist: 'Kenny G', song: 'Going Home (Inst_)' },
  { artist: 'Katie Sky', song: 'Monsters' },
  { artist: 'Katy Perry', song: 'Roar' },
  { artist: 'Kehlani _ Keyshia Cole', song: 'All Me(feat_ Keyshia Cole)' },
  { artist: 'Kiana Ledé', song: 'Show Love' },
  { artist: 'Kiiiu', song: 'Track Maker (慢速版)' },
  { artist: 'Kina_Adriana Proenza', song: 'Can We Kiss Forever_' },
  { artist: 'Kesha', song: 'Blow' },
  { artist: 'Kesha', song: 'TiK ToK' },
  { artist: 'Keyshia Cole', song: 'Fallin Out' },
  { artist: '25時、ナイトコードで。', song: 'キティ' },
  { artist: '2hollis', song: 'poster boy' },
  { artist: '2Someone', song: 'Star Unkind (Lanfranchi & Farina Radio)' },
  { artist: '1K', song: '就忘了吧 (DJAh版)' },
  { artist: '1K', song: '就忘了吧' },
  { artist: '24kGoldn_iann dior', song: 'Mood' },
  { artist: 'AC_DC', song: 'Back In Black' },
  { artist: 'AC_DC', song: 'Hard Times' },
  { artist: 'AC_DC', song: 'Have a Drink on Me' },
  { artist: '2Someone', song: 'Star Unkind (Lanfranchi & Farina Remix)' },
  { artist: '347aidan', song: 'Dancing in My Room' },
  { artist: 'Aaron Smith_Luvli', song: 'Dancin (Krono Remix)' },
  { artist: '陈奕迅', song: '阿猫阿狗' },
  { artist: '陈粒', song: '小半' },
  { artist: '陈粒', song: '虚拟' },
  { artist: '阿肆', song: '热爱105℃的你' },
  { artist: '阿肆', song: '直到你降临' },
  { artist: '阿肆_郭采洁', song: '世界上的另一个我' },
  { artist: '雷军', song: 'Are You OK' },
  { artist: '麗美 (れいみぃ)', song: 'Childhood Days' },
  { artist: '미스티 블루', song: '슈게이저 (Shoegaze)' },
  { artist: '陈绮贞', song: '还是会寂寞' },
  { artist: '陈致逸', song: '爱意' },
  { artist: '陶喆', song: '似曾相识' },
  { artist: 'Alice Merton', song: 'No Roots' },
  { artist: 'Alicia Keys', song: 'If I Aint Got You' },
  { artist: 'Amedeo Serra_Robin Bell', song: 'Deadly Dangerous Love' },
  { artist: 'Alessio Bax', song: 'Zion hört die Wächter singen' },
  { artist: 'Alexandra Stan', song: 'ALL MY PEOPLE' },
  { artist: 'Alexandre Pachabezian', song: 'Una Mattina (Piano Arrangement)' },
  { artist: 'André Kostelanetz', song: 'Can Can' },
  { artist: 'Ann Marie', song: 'Handle It' },
  { artist: 'AOA', song: '짧은 치마 (MINISKIRT) (短裙)' },
  { artist: 'Ameriie_Hattie Menzie', song: 'oneThing' },
  { artist: 'Andora _ RANASOL _ Gweltaz Calori _ Matthew Clanton', song: 'Flicker' },
  { artist: 'Andrew Prahlow', song: 'Travelers encore' },
  { artist: 'Ailee (에일리)', song: 'HEAVEN (Original Version)' },
  { artist: 'Al l bo_Wooshendoo_Miyagi_Эндшпиль', song: '#TAMADA' },
  { artist: 'ALan', song: 'Octopus' },
  { artist: 'AC_DC', song: 'Highway To Hell' },
  { artist: 'AGA', song: 'Wonderful U (Demo Version)' },
  { artist: 'AGA', song: '孤雏' },
  { artist: 'Alan Walker_Iselin Solheim', song: 'Sing Me to Sleep' },
  { artist: 'Alan Walker_Noah Cyrus_Digital Farm Animals_Juliander', song: 'All Falls Down' },
  { artist: 'Alan Walker_Sabrina Carpenter_Farruko', song: 'On My Way' },
  { artist: 'Alan Walker', song: 'Alone' },
  { artist: 'Alan Walker', song: 'Faded' },
  { artist: 'Alan Walker_Au_Ra_Tomine Harket', song: 'Darkside' },
  { artist: '篠螺悠那 (ゆうゆ)_初音未来 (初音ミク)', song: '深海少女' },
  { artist: '結束バンド (纽带乐队)', song: 'あのバンド -Anime Ver_-' },
  { artist: '纯音乐', song: 'shanghaifield (Inst_)' },
  { artist: '秋野花', song: 'Because of' },
  { artist: '秋野花', song: 'ホントノトコロ' },
  { artist: '竹内まりや (竹内玛利亚)', song: 'プラスティック・ラブ (塑料般的爱情)' },
  { artist: '花粥', song: '归去来兮' },
  { artist: '花粥_王胜娚', song: '出山' },
  { artist: '菅野祐悟 (かんの ゆうご)', song: 'Killer (杀手)' },
  { artist: '艾索', song: 'gogo啪啪啪 (铃声)' },
  { artist: '艾索', song: '黑咻狂想曲' },
  { artist: '花澤香菜 (はなざわ かな)', song: 'sweets parade' },
  { artist: '熊雨珂', song: '我知道你 (1_3x)' },
  { artist: '牧野由依', song: 'ふわふわ♪ (轻飘飘♪)' },
  { artist: '璃杏', song: '永遠の孤独 (永远的孤独)' },
  { artist: '清水準一', song: 'Bloom of Youth (风华正茂) (Inst_)' },
  { artist: '热播嗨曲', song: 'Here (DJ版)' },
  { artist: '焦迈奇', song: '我的名字' },
  { artist: '目黒将司 _ tofubeats', song: '星と僕らと (星星与我们) (tofubeats Remix)' },
  { artist: '神山羊', song: 'カトラリー (餐具)' },
  { artist: '神山羊 _ 初音未来', song: 'カトラリー (餐具)' },
  { artist: '生物股长', song: 'SAKURA (樱花)' },
  { artist: '电鸟个灯泡', song: 'YELLOW' },
  { artist: '目黒将司', song: '星と僕らと (星星与我们)' },
  { artist: '郑润泽', song: '遇见' },
  { artist: '郭芯其', song: '冰点' },
  { artist: '郭芯其', song: '学会飞' },
  { artist: '豊崎愛生 _ 高橋李依', song: 'JUMP IN' },
  { artist: '近藤浩治', song: 'おはなばたけ (花田) (花田)' },
  { artist: '郑少秋', song: '笑看风云' },
  { artist: '镜音铃', song: '少女A' },
  { artist: '队长', song: '哪里都是你' },
  { artist: '阿炳', song: '二泉映月' },
  { artist: '鎖那 (さな)', song: '彼女は旅に出る (她踏上了旅程)' },
  { artist: '钉宫理惠', song: '茜色 hometown' },
  { artist: '银临_Aki阿杰', song: '牵丝戏' },
  { artist: '葛东琪', song: '风吹丹顶鹤' },
  { artist: '蔡健雅', song: 'Letting Go (KTV版伴奏)' },
  { artist: '藤間仁', song: '効率' },
  { artist: '萧萧', song: '爱要坦荡荡' },
  { artist: '萧萧', song: '花' },
  { artist: '葛东琪', song: '悬溺' },
  { artist: '西原健一郎 _ Loumina', song: 'embers' },
  { artist: '许嵩', song: '玫瑰花的葬礼' },
  { artist: '谢安琪', song: '其实寂寞' },
  { artist: '藤間仁', song: '繁栄' },
  { artist: '虻川治', song: 'under the moon' },
  { artist: '西原健一郎', song: 'Serendipity' },
  { artist: 'Approaching Nirvana', song: 'You' },
  { artist: 'Chewie Melodies _ Pealeaf', song: 'Color Your Night' },
  { artist: 'Childish Gambino', song: 'Redbone' },
  { artist: 'cici_', song: '把回忆拼好给你' },
  { artist: 'Charli xcx_Lil Yachty', song: 'After the Afterparty (feat_ Lil Yachty)' },
  { artist: 'Charlie Puth_Selena Gomez', song: 'We Dont Talk Anymore' },
  { artist: 'Chester Young_Castion', song: 'PYRO' },
  { artist: 'Cody Fry_Abby Cates', song: 'Things You Said' },
  { artist: 'Coi Leray', song: 'Players' },
  { artist: 'ConcernedApe', song: 'Cloud Country' },
  { artist: 'Cihat Aşkın', song: 'Waltz No_2' },
  { artist: 'Clean Bandit_Jess Glynne', song: 'Rather Be' },
  { artist: 'CMJ _ 好多纯音乐', song: '夜之圆舞曲' },
  { artist: 'BY2', song: '爱的双重魔力' },
  { artist: 'bôa', song: 'Duvet' },
  { artist: 'Cafuné', song: 'Tek It (Sped Up)' },
  { artist: 'br_Cheung', song: 'Call of Silence (钢琴版)' },
  { artist: 'BY2', song: '凑热闹' },
  { artist: 'BY2', song: '我知道' },
  { artist: 'Camila Cabello_Young Thug', song: 'Havana' },
  { artist: 'Carly Rae Jepsen', song: 'Call Me Maybe' },
  { artist: 'Carly Rae Jepsen', song: 'Good Time' },
  { artist: 'Cafuné', song: 'Tek It' },
  { artist: 'Calvin Harris_Dua Lipa', song: 'One Kiss' },
  { artist: 'Calvin Harris_Ellie Goulding', song: 'Outside' },
  { artist: 'Denise King _ Massimo Faraò Trio', song: 'Say You, Say Me' },
  { artist: 'DENKI SAMA', song: 'Moonlight Romance' },
  { artist: 'Diana Boncheva', song: 'Purple Passion' },
  { artist: 'Dawin', song: 'Jumpshot' },
  { artist: 'deca joins', song: '浴室' },
  { artist: 'DeeJay Ghost', song: 'Betty Boop (Ghost Remix)' },
  { artist: 'Dj Blyatman', song: 'Gopnik' },
  { artist: 'DJ OKAWARI', song: 'Flower Dance' },
  { artist: 'DJ OKAWARI', song: 'Luv Letter (情书)' },
  { artist: 'Dido', song: 'Thank You' },
  { artist: 'Dino Sor', song: 'Geisha' },
  { artist: 'Dizzy Dizzo (蔡诗芸)', song: '雨过后的风景' },
  { artist: 'Corinne Bailey Rae', song: 'Like A Star' },
  { artist: 'Cozi Zuehlsdorff_Vicetone', song: 'Way Back' },
  { artist: 'd4vd', song: 'Here With Me' },
  { artist: 'ConcernedApe', song: 'Country Shop' },
  { artist: 'ConcernedApe', song: 'Spring (Wild Horseradish Jam)' },
  { artist: 'ConcernedApe', song: 'Stardew Valley Overture' },
  { artist: 'Dark_Rain', song: 'Xin Đừng Nhấc Máy (Remix)' },
  { artist: 'Darren Hayes', song: 'Creepin up on You' },
  { artist: 'David Campbell', song: 'Cant Take My Eyes Off You' },
  { artist: 'DAISHI DANCE (ダイシ・ダンス)_Cécile Corbel', song: 'Take Me Hand' },
  { artist: 'Dakooka', song: 'Умри, если меня не любишь (Шаг за 20)' },
  { artist: 'Daniel Powter', song: 'Free Loop' },
  { artist: 'azusa', song: 'I wish' },
  { artist: 'azusa', song: 'ひみつの部屋' },
  { artist: 'azusa', song: 'カプチーノ' },
  { artist: 'Avicii', song: 'The Nights' },
  { artist: 'Avicii', song: 'Waiting for Love' },
  { artist: 'AViVA', song: 'GRRRLS' },
  { artist: 'BeatBrothers', song: 'Fight' },
  { artist: 'Beatrich', song: 'Superstar' },
  { artist: 'Beau Young Prince', song: 'Let Go' },
  { artist: 'azusa', song: '夏祭り' },
  { artist: 'B', song: 'complex - Beautiful Lies' },
  { artist: 'beabadoobee', song: 'Coffee' },
  { artist: 'ARI HICKS', song: 'Kiss Me, Kill Me' },
  { artist: 'Ashes Remain', song: 'On My Own' },
  { artist: 'Astels_Renaud Palisseaux', song: 'We Gotta Let Go' },
  { artist: 'Arc North_Krista Marina', song: 'Meant To Be' },
  { artist: 'Aretha Franklin', song: 'No Matter What' },
  { artist: 'Ari Abdul', song: 'BABYDOLL (Speed)' },
  { artist: 'Austin Farwell', song: 'City Lights' },
  { artist: 'Avant', song: 'Everything About You (Album Version)' },
  { artist: 'Avant', song: 'Involve Yourself' },
  { artist: 'Athletics', song: 'III' },
  { artist: 'AURORA', song: 'Cure For Me' },
  { artist: 'AURORA', song: 'Runaway' },
  { artist: 'Breakbot', song: 'Baby Im Yours' },
  { artist: 'Breakbot_Irfane', song: 'Baby Im Yours' },
  { artist: 'Breathe Carolina_Dropgun_Kaleena Zanders', song: 'Sweet Dreams (feat_ Kaleena Zanders)' },
  { artist: 'Blaxy Girls', song: 'If You Feel My Love (Chaow Mix)' },
  { artist: 'BoA', song: 'Only One' },
  { artist: 'Bobby Caldwell', song: 'What You Wont Do For Love' },
  { artist: 'Bruno Mars', song: 'Thats What I Like' },
  { artist: 'Bruno Mars', song: 'Treasure' },
  { artist: 'Bruno Mars', song: 'Versace on the Floor (Single)' },
  { artist: 'Brian Culbertson', song: 'Youll Never Find' },
  { artist: 'Britney Spears', song: 'Toxic' },
  { artist: 'Bruno Mars', song: 'Talking to the Moon' },
  { artist: 'BEYOND', song: '灰色轨迹' },
  { artist: 'Bicep', song: 'Glue' },
  { artist: 'BIGBANG (빅뱅)', song: 'FANTASTIC BABY' },
  { artist: 'Bebe Rexha', song: 'Ferrari' },
  { artist: 'BEYOND', song: '不再犹豫' },
  { artist: 'BEYOND', song: '冷雨夜' },
  { artist: 'Billie Eilish_Khalid', song: 'lovely' },
  { artist: 'BLACKPINK (블랙핑크)', song: 'PLAYING WITH FIRE (Japanese Ver_)' },
  { artist: 'BlankCh3ck_Nathaniel Davis', song: 'Soft Lips' },
  { artist: 'BIGBANG (빅뱅)', song: 'IF YOU (JPN Ver_)' },
  { artist: 'Bigubar', song: 'One More Light (instrumental)' },
  { artist: 'Billie Eilish', song: 'LUNCH' },
];

const filteredMusic = computed(() => {
  const q = musicQuery.value.trim().toLowerCase();
  if (!q) return musicItems;
  return musicItems.filter(m =>
    m.artist.toLowerCase().includes(q) ||
    m.song.toLowerCase().includes(q)
  );
});



const navItems = [
  { id: 'home', label: 'Nexus', page: 'home', keywords: '首页 主页 home' },
  { id: 'web', label: '网页', page: 'web', keywords: 'web 网站' },
  { id: 'software', label: '软件', page: 'software', keywords: 'software 应用 程序' },
  { id: 'music', label: '音乐', page: 'music', keywords: 'music 音乐' },
  { id: 'artist', label: '画师', page: 'artist', keywords: '画师 插画 绘画 艺术' },
];

const categoryItems = [
  { id: 'web', label: '知识学习', keywords: '学习 知识 教程 编程', page: 'web' },
  { id: 'web', label: '实用工具', keywords: '工具 实用 硬件 代码', page: 'web' },
  { id: 'web', label: '灵感参考', keywords: '灵感 参考 设计 摄影 绘画', page: 'web' },
  { id: 'web', label: '影音娱乐', keywords: '影音 娱乐 音乐 动漫 钢琴', page: 'web' },
  { id: 'web', label: '游戏辅助', keywords: '游戏 辅助 模组 mod', page: 'web' },
];

const toolItems = [
  { id: 'web', label: '菜鸟教程', desc: '编程基础在线教程', keywords: '编程 前端 后端 教程 runoob', page: 'web' },
  { id: 'web', label: 'Codédex', desc: '交互式编程学习平台', keywords: '编程 学习 交互 codedex', page: 'web' },
  { id: 'software', label: '图吧工具箱', desc: '硬件检测与系统工具合集', keywords: '硬件 检测 系统 工具 tbtool', page: 'software' },
  { id: 'web', label: 'GitHub', desc: '开源代码托管平台', keywords: '代码 开源 托管 仓库', page: 'web' },
  { id: 'web', label: 'USTC 网络测速', desc: '校园网速度测试', keywords: '网络 速度 测试 带宽 科大', page: 'web' },
  { id: 'web', label: 'Coverbox', desc: '专辑封面查找', keywords: '封面 专辑 cover 查找', page: 'web' },
  { id: 'web', label: 'Pinterest', desc: '视觉灵感与创意收集', keywords: '灵感 创意 图片 视觉', page: 'web' },
  { id: 'web', label: 'Pixiv', desc: '日本插画艺术社区', keywords: '插画 画师 二次元 日本 art', page: 'web' },
  { id: 'web', label: 'Bodies in Motion', desc: '人体动态参考摄影', keywords: '人体 动态 参考 摄影 姿势', page: 'web' },
  { id: 'web', label: 'Vu Toka', desc: '摄影师人像作品参考', keywords: '摄影 人像 作品 参考 摄影师', page: 'web' },
  { id: 'web', label: '500px', desc: '全球摄影师作品社区', keywords: '摄影 社区 作品 照片', page: 'web' },
  { id: 'web', label: '蜜柑计划', desc: '动漫资源字幕下载', keywords: '动漫 字幕 下载 资源 mikan', page: 'web' },
  { id: 'web', label: 'MidiShow', desc: 'MIDI 音乐资源分享', keywords: 'MIDI 音乐 乐谱 midi', page: 'web' },
  { id: 'web', label: '天天钢琴', desc: '钢琴谱与免费在线课程', keywords: '钢琴 谱 学习 课程 piastudy', page: 'web' },
  { id: 'web', label: 'Nexus Mods', desc: '全球最大游戏模组社区', keywords: '游戏 模组 mod 社区', page: 'web' },
  { id: 'web', label: 'Muse Dash 模组社区', desc: '二次元音游与模组', keywords: '音游 模组 muse dash mdmc', page: 'web' },
  { id: 'software', label: 'LocalSend', desc: '跨平台局域网文件传输', keywords: '文件 传输 局域网 localsend', page: 'software' },
  { id: 'software', label: 'Live2D Cubism', desc: '2D 角色动画制作软件', keywords: 'live2d 动画 角色 2d', page: 'software' },
  { id: 'software', label: 'Eagle', desc: '设计师素材管理工具', keywords: '素材 管理 设计 eagle', page: 'software' },
  { id: 'software', label: 'Steam', desc: '全球最大 PC 游戏平台', keywords: 'steam 游戏 平台 pc', page: 'software' },
  { id: 'software', label: 'PaintTool SAI2', desc: '轻量级数码绘画软件', keywords: 'sai 绘画 画图 数码', page: 'software' },
  { id: 'software', label: 'PotPlayer', desc: '全能视频与音乐播放器', keywords: '播放器 视频 音乐 potplayer', page: 'software' },
  { id: 'software', label: 'Topaz Photo AI', desc: 'AI 图像画质增强软件', keywords: 'topaz ai 图像 画质 增强', page: 'software' },
  { id: 'software', label: 'NVIDIA App', desc: 'NVIDIA 驱动与游戏优化', keywords: 'nvidia 显卡 驱动 优化', page: 'software' },
  { id: 'software', label: 'Venera', desc: '漫画阅读与下载工具', keywords: '漫画 阅读 下载 comic manga', page: 'software' },
  { id: 'software', label: 'LiteMonitor', desc: '轻量级系统监控工具', keywords: '监控 系统 硬件 litemonitor', page: 'software' },
  { id: 'software', label: 'Time Tracking', desc: '时间追踪与效率管理', keywords: '时间 追踪 效率 管理 time', page: 'software' },
  { id: 'software', label: 'FileConverter', desc: '文件格式批量转换', keywords: '文件 转换 格式 fileconverter', page: 'software' },
  { id: 'software', label: 'Sequator', desc: '天文摄影堆栈合成软件', keywords: '天文 摄影 堆栈 星空 sequator', page: 'software' },
  { id: 'software', label: 'DaVinci Resolve', desc: '专业视频剪辑与调色软件', keywords: '视频 剪辑 调色 达芬奇 resolve', page: 'software' },
  { id: 'software', label: 'Studio One 7', desc: '专业数字音频工作站', keywords: '音频 录音 混音 studio one', page: 'software' },
  { id: 'software', label: 'OBS Studio', desc: '免费直播与录屏软件', keywords: '直播 录屏 推流 obs', page: 'software' },
  { id: 'software', label: 'OpenCode', desc: 'AI 编程助手工具', keywords: 'ai 编程 助手 opencode', page: 'software' },
  { id: 'software', label: 'ImageGlass', desc: '轻量级图片查看器', keywords: '图片 查看 浏览器 imageglass', page: 'software' },
  { id: 'software', label: 'Internet Download Manager', desc: '高速下载管理器', keywords: '下载 管理 idm 加速', page: 'software' },
  { id: 'software', label: 'Plain Craft Launcher 2', desc: '我的世界启动器', keywords: 'minecraft 我的世界 启动器 pcl2', page: 'software' },
  { id: 'software', label: 'VS Code', desc: '轻量级代码编辑器', keywords: 'vscode 代码 编辑器 编程', page: 'software' },
  { id: 'software', label: 'PyCharm', desc: 'Python 专业 IDE', keywords: 'pycharm python ide 编程', page: 'software' },
  { id: 'software', label: '盘搜', desc: '网盘资源搜索工具', keywords: '网盘 搜索 资源 盘搜 pansou', page: 'software' },
  { id: 'software', label: 'DownKyi', desc: 'Bilibili 视频下载工具', keywords: 'bilibili 视频 下载 downkyi', page: 'software' },
  { id: 'software', label: 'Pixiv Batch Downloader', desc: 'Pixiv 批量下载工具', keywords: 'pixiv 批量 下载 图片', page: 'software' },
  { id: 'software', label: 'KCC', desc: 'Kindle 漫画格式转换', keywords: 'kindle 漫画 格式 转换 kcc', page: 'software' },
];

const allToolItems = [
  { label: '菜鸟教程', desc: '编程基础在线教程', icon: '📘', url: 'https://www.runoob.com' },
  { label: 'Codédex', desc: '交互式编程学习平台', icon: '💻', url: 'https://www.codedex.io' },
  { label: 'GitHub', desc: '开源代码托管平台', icon: '💻', url: 'https://github.com' },
  { label: 'USTC 网络测速', desc: '校园网速度测试', icon: '🌐', url: 'https://test.ustc.edu.cn/' },
  { label: 'Coverbox', desc: '专辑封面查找', icon: '🎨', url: 'https://coverbox.henry-hu.com/' },
  { label: 'Pinterest', desc: '视觉灵感与创意收集', icon: '📌', url: 'https://www.pinterest.com' },
  { label: 'Pixiv', desc: '日本插画艺术社区', icon: '🖼️', url: 'https://www.pixiv.net' },
  { label: 'Bodies in Motion', desc: '人体动态参考摄影', icon: '🏃', url: 'https://www.bodiesinmotion.photo' },
  { label: 'Vu Toka', desc: '摄影师人像作品参考', icon: '📷', url: 'https://vutoka.com/' },
  { label: '500px', desc: '全球摄影师作品社区', icon: '📸', url: 'https://500px.com.cn' },
  { label: 'Nexus Mods', desc: '全球最大游戏模组社区', icon: '🎮', url: 'https://www.nexusmods.com' },
  { label: 'Muse Dash 模组社区', desc: '二次元音游与模组', icon: '🎵', url: 'https://mdmc.moe/charts' },
  { label: '蜜柑计划', desc: '动漫资源字幕下载', icon: '🍊', url: 'https://mikanani.me' },
  { label: 'MidiShow', desc: 'MIDI 音乐资源分享', icon: '🎹', url: 'https://www.midishow.com' },
  { label: '天天钢琴', desc: '钢琴谱与免费在线课程', icon: '🎹', url: 'https://piastudy.com/' },
  { label: '图吧工具箱', desc: '硬件检测与系统工具合集', icon: '🔧', url: 'https://www.tbtool.cn' },
  { label: 'LocalSend', desc: '跨平台局域网文件传输', icon: '📤', url: 'https://localsend.org' },
  { label: 'Live2D Cubism', desc: '2D 角色动画制作软件', icon: '🎭', url: 'https://www.live2d.com' },
  { label: 'PaintTool SAI2', desc: '轻量级数码绘画软件', icon: '🎨', url: 'https://www.systemax.jp/en/sai/' },
  { label: 'PotPlayer', desc: '全能视频与音乐播放器', icon: '📺', url: 'https://potplayer.daum.net' },
  { label: 'Topaz Photo AI', desc: 'AI 图像画质增强软件', icon: '🤖', url: 'https://www.topazlabs.com/topaz-photo-ai' },
  { label: 'NVIDIA App', desc: 'NVIDIA 驱动与游戏优化', icon: '🟢', url: 'https://www.nvidia.com/en-us/software/nvidia-app/' },
  { label: 'LiteMonitor', desc: '轻量级系统监控工具', icon: '📊', url: 'https://github.com/Diorser/LiteMonitor' },
  { label: 'Time Tracking', desc: '时间追踪与效率管理', icon: '⏱️', url: 'https://github.com/Ceceliaee/time-tracking' },
  { label: 'FileConverter', desc: '文件格式批量转换', icon: '🔄', url: 'https://github.com/Tichau/FileConverter' },
  { label: 'Sequator', desc: '天文摄影堆栈合成软件', icon: '✨', url: 'https://sites.google.com/view/sequator' },
  { label: 'DaVinci Resolve', desc: '专业视频剪辑与调色软件', icon: '🎬', url: 'https://www.blackmagicdesign.com/products/davinciresolve' },
  { label: 'Studio One 7', desc: '专业数字音频工作站', icon: '🎵', url: 'https://www.presonus.com/en-us/studio-one/' },
  { label: 'OBS Studio', desc: '免费直播与录屏软件', icon: '📹', url: 'https://obsproject.com' },
  { label: 'OpenCode', desc: 'AI 编程助手工具', icon: '🤖', url: 'https://opencode.ai' },
  { label: 'ImageGlass', desc: '轻量级图片查看器', icon: '🖼️', url: 'https://imageglass.org' },
  { label: 'VS Code', desc: '轻量级代码编辑器', icon: '📝', url: 'https://code.visualstudio.com' },
  { label: 'PyCharm', desc: 'Python 专业 IDE', icon: '🐍', url: 'https://www.jetbrains.com/pycharm/' },
  { label: 'Eagle', desc: '设计师素材管理工具', icon: '🦅', url: 'https://eagle.cool' },
  { label: 'Venera', desc: '漫画阅读与下载工具', icon: '📖', url: 'https://github.com/venera-app/venera' },
  { label: '盘搜', desc: '网盘资源搜索工具', icon: '🔍', url: 'https://github.com/fish2018/pansou' },
  { label: 'DownKyi', desc: 'Bilibili 视频下载工具', icon: '📥', url: 'https://github.com/yaobiao131/downkyicore' },
  { label: 'Pixiv Batch Downloader', desc: 'Pixiv 批量下载工具', icon: '🖼️', url: 'https://github.com/xuejianxianzun/PixivBatchDownloader' },
  { label: 'KCC', desc: 'Kindle 漫画格式转换', icon: '📱', url: 'https://github.com/ciromattia/kcc' },
  { label: 'Internet Download Manager', desc: '高速下载管理器', icon: '⚡', url: 'https://www.internetdownloadmanager.com' },
  { label: 'Steam', desc: '全球最大 PC 游戏平台', icon: '🎮', url: 'https://store.steampowered.com' },
  { label: 'Plain Craft Launcher 2', desc: '我的世界启动器', icon: '⛏️', url: 'https://github.com/Hex-Dragon/PCL2' },
];

const allSearchItems = computed(() => {
  const q = searchQuery.value.trim().toLowerCase();
  if (!q) return [];

  const match = (item) => {
    const label = item.label.toLowerCase();
    const kw = (item.keywords || '').toLowerCase();
    const desc = (item.desc || '').toLowerCase();
    return label.includes(q) || kw.includes(q) || desc.includes(q);
  };

  const all = [...toolItems];
  const exact = all.filter(item => item.label.toLowerCase() === q);
  const exactLabels = exact.map(e => e.label);
  const fuzzy = all.filter(item =>
    !exactLabels.includes(item.label) &&
    match(item)
  );
  return [...exact, ...fuzzy];
});

function goTo(item) {
  currentPage.value = item.page || item.id;
  searchQuery.value = '';
}

function doSearch() {
  if (allSearchItems.value.length) {
    goTo(allSearchItems.value[0]);
  }
}

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

function onImgError(e) {
  e.target.style.display = 'none';
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
  margin-bottom: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.search-wrapper {
  display: flex;
  width: 100%;
  max-width: 400px;
  border-radius: 999px;
  border: 1px solid rgba(180, 180, 180, 0.3);
  background: rgba(50, 50, 50, 0.5);
  overflow: hidden;
}

.page:not(.dark) .search-wrapper {
  background: #ffffff;
  border-color: rgba(0, 0, 0, 0.15);
}

.search-input {
  flex: 1;
  padding: 10px 16px;
  border: none;
  background: transparent;
  color: #e0e0e0;
  font-size: 0.95rem;
  outline: none;
  box-sizing: border-box;
}

.search-input::placeholder {
  color: #888888;
}

.page:not(.dark) .search-input {
  color: #000000;
}

.search-btn {
  width: 42px;
  border: none;
  background: transparent;
  color: #e0e0e0;
  cursor: pointer;
  font-size: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.page:not(.dark) .search-btn {
  color: #000000;
}

.search-results {
  margin-top: 8px;
  border-radius: 8px;
  overflow: hidden;
  width: 100%;
  max-width: 400px;
}

.search-result-item {
  padding: 10px 16px;
  cursor: pointer;
  font-size: 0.95rem;
  background: rgba(50, 50, 50, 0.8);
  transition: background 0.15s;
}

.search-result-item:hover {
  background: rgba(180, 180, 180, 0.15);
}

.page:not(.dark) .search-result-item {
  background: #f0f0f0;
}

.page:not(.dark) .search-result-item:hover {
  background: #e0e0e0;
}

.content {
  flex: 1;
  min-width: 0;
}

.web-page {
  text-align: left;
}

.web-page h2 {
  color: inherit;
}

.category-section {
  margin-bottom: 32px;
}

.all-cards {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  justify-content: center;
}

.software-page {
  text-align: left;
}

.software-page h2 {
  color: inherit;
}

.artist-page {
  text-align: left;
}

.artist-search-bar {
  display: flex;
  justify-content: center;
  margin-bottom: 24px;
}

.artist-search-wrapper {
  display: flex;
  align-items: center;
  background: rgba(180,180,180,0.12);
  border-radius: 999px;
  padding: 0 4px 0 16px;
  width: 100%;
  max-width: 400px;
}

.page.light .artist-search-wrapper {
  background: rgba(0,0,0,0.06);
}

.artist-search-input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  color: inherit;
  font-size: 0.95rem;
  padding: 10px 0;
}

.artist-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
  gap: 24px;
}

.artist-card {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px 24px 20px;
  border-radius: 16px;
  background: rgba(180, 180, 180, 0.08);
  border: 1px solid rgba(180, 180, 180, 0.12);
  transition: background 0.2s;
}

.artist-card:hover {
  background: rgba(180, 180, 180, 0.15);
}

.page:not(.dark) .artist-card {
  background: rgba(0, 0, 0, 0.04);
  border-color: rgba(0, 0, 0, 0.08);
}

.page:not(.dark) .artist-card:hover {
  background: rgba(0, 0, 0, 0.07);
}

.artist-header {
  display: flex;
  align-items: flex-start;
  gap: 20px;
}

.artist-avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  flex-shrink: 0;
}

.artist-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.artist-info {
  flex: 1;
  min-width: 0;
}

.artist-name {
  margin: 0 0 10px;
  font-size: 1.5rem;
  color: inherit;
}

.artist-links {
  display: flex;
  gap: 12px;
}

.pixiv-link {
  display: inline-block;
  padding: 4px 14px;
  border-radius: 20px;
  font-size: 0.85rem;
  text-decoration: none;
  transition: opacity 0.2s;
  background: #0096fa;
  color: #fff;
}

.pixiv-link:hover {
  opacity: 0.8;
}

.artist-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 10px;
}

.tag {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 0.78rem;
  background: rgba(128, 128, 128, 0.18);
  color: inherit;
}

.page.light .tag {
  background: rgba(0, 0, 0, 0.08);
}

.music-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 10px;
}

.music-card {
  padding: 12px 16px;
  border-radius: 10px;
  background: rgba(180, 180, 180, 0.06);
  border: 1px solid rgba(180, 180, 180, 0.1);
  transition: background 0.2s;
  cursor: pointer;
}

.music-card:hover {
  background: rgba(180, 180, 180, 0.12);
}

.page:not(.dark) .music-card {
  background: rgba(0, 0, 0, 0.03);
  border-color: rgba(0, 0, 0, 0.06);
}

.page:not(.dark) .music-card:hover {
  background: rgba(0, 0, 0, 0.06);
}

.music-song {
  font-size: 0.95rem;
  font-weight: 600;
  color: inherit;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.music-artist {
  font-size: 0.8rem;
  opacity: 0.6;
  color: inherit;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-top: 2px;
}

.toast {
  position: fixed;
  z-index: 9999;
  transform: translate(-50%, -100%);
  padding: 6px 16px;
  border-radius: 8px;
  font-size: 0.85rem;
  background: rgba(0,0,0,0.75);
  color: #fff;
  pointer-events: none;
  animation: floatUp 1.2s ease-out forwards;
}

.page.light .toast {
  background: rgba(0,0,0,0.7);
}

@keyframes floatUp {
  0%   { opacity: 1; transform: translate(-50%, -100%); }
  100% { opacity: 0; transform: translate(-50%, -200%); }
}

.artist-works {
  display: flex;
  gap: 10px;
}

.work-thumb {
  flex: 1;
  aspect-ratio: 3 / 4;
  overflow: hidden;
  border-radius: 10px;
  background: rgba(128, 128, 128, 0.15);
  cursor: pointer;
}

.work-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.lightbox {
  position: fixed;
  z-index: 999;
  inset: 0;
  background: rgba(0,0,0,0.85);
  display: flex;
  align-items: center;
  justify-content: center;
}

.lightbox-img {
  max-width: 90vw;
  max-height: 90vh;
  object-fit: contain;
  border-radius: 8px;
}

.page-transition {
  animation: fadeIn 0.25s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}


.page-heading {
  margin: 0;
  font-size: 2rem;
}

.category-title {
  font-size: 1.8rem;
  font-weight: 700;
  margin: 0 0 16px;
}

.tool-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.tool-card {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 18px;
  border-radius: 8px;
  background: rgba(180, 180, 180, 0.1);
  cursor: pointer;
  transition: background 0.2s;
  text-decoration: none;
  color: inherit;
}

.tool-card:hover {
  background: rgba(180, 180, 180, 0.2);
}

.page:not(.dark) .tool-card {
  background: rgba(0, 0, 0, 0.06);
}

.page:not(.dark) .tool-card:hover {
  background: rgba(0, 0, 0, 0.1);
}

.tool-icon {
  font-size: 1.2rem;
}

.tool-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.tool-name {
  font-size: 0.95rem;
  font-weight: 600;
}

.tool-desc {
  font-size: 0.8rem;
  opacity: 0.65;
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
  justify-content: space-between;
  }
}
</style>
