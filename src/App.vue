<template>
  <div :class="['page', { dark: isDark }]">
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
            <input class="search-input" type="text" placeholder="你所想" v-model="searchQuery" @keydown.enter="doSearch" />
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
                  <img :src="a.avatar" :alt="a.name" />
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
                    <img :src="w" :alt="a.name + ' work ' + (wi+1)" loading="lazy" />
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
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const isDark = ref(true);
const currentPage = ref('home');
const searchQuery = ref('');
const previewImg = ref('');
const artistQuery = ref('');

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
    { name: 'Noyu', avatar: '/my-navigation-site/images/artists/noyu.jpg', pixiv: 'https://www.pixiv.net/users/26040235', works: ['/my-navigation-site/images/artworks/noyu/1.jpg','/my-navigation-site/images/artworks/noyu/2.jpg','/my-navigation-site/images/artworks/noyu/3.jpg'] , tags: '少女, 厚涂, 奇幻, 氛围, 人外, 哥特, 色彩'},
    { name: '望月けい', avatar: '/my-navigation-site/images/artists/mochizuki_kei.jpg', pixiv: 'https://www.pixiv.net/users/1193008', works: ['/my-navigation-site/images/artworks/mochizuki_kei/1.jpg','/my-navigation-site/images/artworks/mochizuki_kei/2.jpg','/my-navigation-site/images/artworks/mochizuki_kei/3.jpg'] , tags: '厚涂, 帅气, 风格化, 原创, 个性, 三丽鸥, 设计'},
    { name: 'neco', avatar: '/my-navigation-site/images/artists/neco.jpg', pixiv: 'https://www.pixiv.net/users/777703', works: ['/my-navigation-site/images/artworks/neco/1.jpg','/my-navigation-site/images/artworks/neco/2.jpg','/my-navigation-site/images/artworks/neco/3.jpg'] , tags: '机甲, 科幻, 重装, 少女, 武器, 军武, 设计'},
    { name: '室埴 ポコ', avatar: '/my-navigation-site/images/artists/murohani_poco.jpg', pixiv: 'https://www.pixiv.net/users/76266', works: ['/my-navigation-site/images/artworks/murohani_poco/1.jpg','/my-navigation-site/images/artworks/murohani_poco/2.jpg','/my-navigation-site/images/artworks/murohani_poco/3.jpg'] , tags: '机甲, 机械, 兽耳, 狐狸, 金发, 尾巴, 武器'},
    { name: '赤倉＠画集発売中', avatar: '/my-navigation-site/images/artists/akakura.jpg', pixiv: 'https://www.pixiv.net/users/882569', works: ['/my-navigation-site/images/artworks/akakura/1.jpg','/my-navigation-site/images/artworks/akakura/2.jpg','/my-navigation-site/images/artworks/akakura/3.jpg'] , tags: '少女, 可爱, 天使, 恶魔, 制服, 绚丽, 色彩'},
    { name: 'Anmi@画集発売中', avatar: '/my-navigation-site/images/artists/anmi.jpg', pixiv: 'https://www.pixiv.net/users/212801', works: ['/my-navigation-site/images/artworks/anmi/1.jpg','/my-navigation-site/images/artworks/anmi/2.jpg','/my-navigation-site/images/artworks/anmi/3.jpg'] , tags: '少女, 唯美, 百合, 治愈, 色彩, 花, 柔光'},
    { name: 'アシマ / Ashima', avatar: '/my-navigation-site/images/artists/ashima.jpg', pixiv: 'https://www.pixiv.net/users/2642047', works: ['/my-navigation-site/images/artworks/ashima/1.jpg','/my-navigation-site/images/artworks/ashima/2.jpg','/my-navigation-site/images/artworks/ashima/3.jpg'] , tags: '少女, 幻想, 华丽, 花, 制服, 优雅, 可爱'},
    { name: 'モ誰', avatar: '/my-navigation-site/images/artists/mosui.jpg', pixiv: 'https://www.pixiv.net/users/1878082', works: ['/my-navigation-site/images/artworks/mosui/1.jpg','/my-navigation-site/images/artworks/mosui/2.jpg','/my-navigation-site/images/artworks/mosui/3.jpg'] , tags: '厚涂, 韩系, 精致, 美少女, 笑容, 角色, 华丽'},
    { name: 'イコモチ', avatar: '/my-navigation-site/images/artists/icomochi.jpg', pixiv: 'https://www.pixiv.net/users/801146', works: ['/my-navigation-site/images/artworks/icomochi/1.jpg','/my-navigation-site/images/artworks/icomochi/2.jpg','/my-navigation-site/images/artworks/icomochi/3.jpg'] , tags: '可爱, VTuber, hololive, FUWAMOCO, 同人, 兽耳, 萌'},
    { name: 'あるてら', avatar: '/my-navigation-site/images/artists/arutera.jpg', pixiv: 'https://www.pixiv.net/users/483730', works: ['/my-navigation-site/images/artworks/arutera/1.jpg','/my-navigation-site/images/artworks/arutera/2.jpg','/my-navigation-site/images/artworks/arutera/3.jpg'] , tags: '少女, VTuber, 礼装, 长发, 色彩, 优雅, 动态'},
    { name: '鬼针草', avatar: '/my-navigation-site/images/artists/guizhencao.jpg', pixiv: 'https://www.pixiv.net/users/6049901', works: ['/my-navigation-site/images/artworks/guizhencao/1.jpg','/my-navigation-site/images/artworks/guizhencao/2.jpg','/my-navigation-site/images/artworks/guizhencao/3.jpg'] , tags: '少女, 白丝, 制服, 黑丝, 短髮, 明日方舟, 绝对领域'},
    { name: 'ヒトこもる', avatar: '/my-navigation-site/images/artists/hitokomoru.jpg', pixiv: 'https://www.pixiv.net/users/30837811', works: ['/my-navigation-site/images/artworks/hitokomoru/1.jpg','/my-navigation-site/images/artworks/hitokomoru/2.jpg','/my-navigation-site/images/artworks/hitokomoru/3.jpg'] , tags: '少女, 可爱, 日常, 治愈, 白髮, 兽耳, 笑容'},
    { name: 'wlop', avatar: '/my-navigation-site/images/artists/wlop.jpg', pixiv: 'https://www.pixiv.net/users/2188232', works: ['/my-navigation-site/images/artworks/wlop/1.jpg','/my-navigation-site/images/artworks/wlop/2.jpg','/my-navigation-site/images/artworks/wlop/3.jpg'] , tags: '厚涂, 奇幻, 氛围, 光影, 史诗, 场景, 原创'},
    { name: 'ASK', avatar: '/my-navigation-site/images/artists/ask.jpg', pixiv: 'https://www.pixiv.net/users/1980643', works: ['/my-navigation-site/images/artworks/ask/1.jpg','/my-navigation-site/images/artworks/ask/2.jpg','/my-navigation-site/images/artworks/ask/3.jpg'] , tags: '少女, 唯美, FGO, 和风, 礼装, 优雅, 氛围'},
    { name: 'rurudo', avatar: '/my-navigation-site/images/artists/rurudo.jpg', pixiv: 'https://www.pixiv.net/users/25760573', works: ['/my-navigation-site/images/artworks/rurudo/1.jpg','/my-navigation-site/images/artworks/rurudo/2.jpg','/my-navigation-site/images/artworks/rurudo/3.jpg'] , tags: '萝莉, 可爱, 少女, 性感, 兔女郎, 萌, 制服'},
    { name: 'しらたま❄', avatar: '/my-navigation-site/images/artists/shiratama.jpg', pixiv: 'https://www.pixiv.net/users/705370', works: ['/my-navigation-site/images/artworks/shiratama/1.jpg','/my-navigation-site/images/artworks/shiratama/2.jpg','/my-navigation-site/images/artworks/shiratama/3.jpg'] , tags: '萝莉, 可爱, 少女, 治愈, 星空, 天使, 梦幻'},
    { name: 'こうこうや', avatar: '/my-navigation-site/images/artists/kokoya.jpg', pixiv: 'https://www.pixiv.net/users/118616623', works: ['/my-navigation-site/images/artworks/kokoya/1.jpg','/my-navigation-site/images/artworks/kokoya/2.jpg','/my-navigation-site/images/artworks/kokoya/3.jpg'] , tags: '少女, 东方, 背景, 风景, 女仆, 幻想, 可爱'},
    { name: '落舟Pile', avatar: '/my-navigation-site/images/artists/luozhou_pile.jpg', pixiv: 'https://www.pixiv.net/users/21304996', works: ['/my-navigation-site/images/artworks/luozhou_pile/1.jpg','/my-navigation-site/images/artworks/luozhou_pile/2.jpg','/my-navigation-site/images/artworks/luozhou_pile/3.jpg'] , tags: '明日方舟, 少女, 插画, 药屋, 厚涂, 色彩, 角色'},
    { name: 'mojo', avatar: '/my-navigation-site/images/artists/mojo.jpg', pixiv: 'https://www.pixiv.net/users/94576902', works: ['/my-navigation-site/images/artworks/mojo/1.jpg','/my-navigation-site/images/artworks/mojo/2.jpg','/my-navigation-site/images/artworks/mojo/3.jpg'] , tags: '蔚蓝档案, 机甲, 科幻, 校园, 偶像, 机械, 少女'},
    { name: 'Nbrush19', avatar: '/my-navigation-site/images/artists/nbrush19.jpg', pixiv: 'https://www.pixiv.net/users/44895546', works: ['/my-navigation-site/images/artworks/nbrush19/1.jpg','/my-navigation-site/images/artworks/nbrush19/2.jpg','/my-navigation-site/images/artworks/nbrush19/3.jpg'] , tags: '明日方舟, 少女, 插画, Fate, 厚涂, 色彩, 优美'},
    { name: '富士やま', avatar: '/my-navigation-site/images/artists/fujiyama.jpg', pixiv: 'https://www.pixiv.net/users/9343974', works: ['/my-navigation-site/images/artworks/fujiyama/1.jpg','/my-navigation-site/images/artworks/fujiyama/2.jpg','/my-navigation-site/images/artworks/fujiyama/3.jpg'] , tags: '少女, 游戏, 可爱, 猫耳, 女仆, 厚涂, 色彩'},
    { name: 'ふぇありぃあい', avatar: '/my-navigation-site/images/artists/fairy_eye.jpg', pixiv: 'https://www.pixiv.net/users/1055457', works: ['/my-navigation-site/images/artworks/fairy_eye/1.jpg','/my-navigation-site/images/artworks/fairy_eye/2.jpg','/my-navigation-site/images/artworks/fairy_eye/3.jpg'] , tags: 'hololive, VTuber, 可爱, 少女, 女仆, 同人, 治愈'},
    { name: '宮坂みゆ', avatar: '/my-navigation-site/images/artists/miyasaka_miyu.jpg', pixiv: 'https://www.pixiv.net/users/839617', works: ['/my-navigation-site/images/artworks/miyasaka_miyu/1.jpg','/my-navigation-site/images/artworks/miyasaka_miyu/2.jpg','/my-navigation-site/images/artworks/miyasaka_miyu/3.jpg'] , tags: '少女, 可爱, 双马尾, 同人, 粉发, 萝莉, 童话'},
    { name: '飴玉コン', avatar: '/my-navigation-site/images/artists/amedama_kon.jpg', pixiv: 'https://www.pixiv.net/users/1992163', works: ['/my-navigation-site/images/artworks/amedama_kon/1.jpg','/my-navigation-site/images/artworks/amedama_kon/2.jpg','/my-navigation-site/images/artworks/amedama_kon/3.jpg'] , tags: '少女, 百合, 厚涂, 萝莉, 白髮, 泳装, 双马尾'},
    { name: 'きょくちょ', avatar: '/my-navigation-site/images/artists/kyockcho.jpg', pixiv: 'https://www.pixiv.net/users/22782', works: ['/my-navigation-site/images/artworks/kyockcho/1.jpg','/my-navigation-site/images/artworks/kyockcho/2.jpg','/my-navigation-site/images/artworks/kyockcho/3.jpg'] , tags: '少女, 可爱, 女仆, 漫画, 制服, 校园, 萝莉'},
    { name: '秋タケ', avatar: '/my-navigation-site/images/artists/akitake.jpg', pixiv: 'https://www.pixiv.net/users/21076169', works: ['/my-navigation-site/images/artworks/akitake/1.jpg','/my-navigation-site/images/artworks/akitake/2.jpg','/my-navigation-site/images/artworks/akitake/3.jpg'] , tags: '蔚蓝档案, 少女, 可爱, 笑容, 校园, 制服, 阳光'},
    { name: 'ぱるふぇいと', avatar: '/my-navigation-site/images/artists/parfait.jpg', pixiv: 'https://www.pixiv.net/users/1179457', works: ['/my-navigation-site/images/artworks/parfait/1.jpg','/my-navigation-site/images/artworks/parfait/2.jpg','/my-navigation-site/images/artworks/parfait/3.jpg'] , tags: '白发, 少女, 萝莉, 可爱, 幻想, 梦幻, 天使'},
    { name: '薄藍', avatar: '/my-navigation-site/images/artists/usui.jpg', pixiv: 'https://www.pixiv.net/users/38276220', works: ['/my-navigation-site/images/artworks/usui/1.jpg','/my-navigation-site/images/artworks/usui/2.jpg','/my-navigation-site/images/artworks/usui/3.jpg'] , tags: '少女, FGO, 幻想, 可爱, 天使, 花, 色彩'},
    { name: '種乃なかみ', avatar: '/my-navigation-site/images/artists/taneno_nakami.jpg', pixiv: 'https://www.pixiv.net/users/12823252', works: ['/my-navigation-site/images/artworks/taneno_nakami/1.jpg','/my-navigation-site/images/artworks/taneno_nakami/2.jpg','/my-navigation-site/images/artworks/taneno_nakami/3.jpg'] , tags: '少女, 百合, 同人, 可爱, 制服, 马尾, 日常'},
    { name: 'こんぺ伊藤', avatar: '/my-navigation-site/images/artists/konpe_ito.jpg', pixiv: 'https://www.pixiv.net/users/17777967', works: ['/my-navigation-site/images/artworks/konpe_ito/1.jpg','/my-navigation-site/images/artworks/konpe_ito/2.jpg','/my-navigation-site/images/artworks/konpe_ito/3.jpg'] , tags: '少女, 女仆, 猫耳, 双马尾, 白髮, 护士, 可爱'},
    { name: 'いずみななせ', avatar: '/my-navigation-site/images/artists/izumi_nanase.jpg', pixiv: 'https://www.pixiv.net/users/7145395', works: ['/my-navigation-site/images/artworks/izumi_nanase/1.jpg','/my-navigation-site/images/artworks/izumi_nanase/2.jpg','/my-navigation-site/images/artworks/izumi_nanase/3.jpg'] , tags: 'VTuber, 少女, 委托, VRChat, 猫耳, 可爱, 立绘'},
    { name: 'にんげんまめ', avatar: '/my-navigation-site/images/artists/ningen_mame.jpg', pixiv: 'https://www.pixiv.net/users/38297201', works: ['/my-navigation-site/images/artworks/ningen_mame/1.jpg','/my-navigation-site/images/artworks/ningen_mame/2.jpg','/my-navigation-site/images/artworks/ningen_mame/3.jpg'] , tags: '蔚蓝档案, 少女, 校园, 偶像, 制服, 可爱, 青春'},
    { name: 'Dudrinm', avatar: '/my-navigation-site/images/artists/dudrinm.jpg', pixiv: 'https://www.pixiv.net/users/94061649', works: ['/my-navigation-site/images/artworks/dudrinm/1.jpg','/my-navigation-site/images/artworks/dudrinm/2.jpg','/my-navigation-site/images/artworks/dudrinm/3.jpg'] , tags: '少女, EVA, 同人, 奇幻, 鸣潮, 色彩, 原创'},
    { name: 'Noah', avatar: '/my-navigation-site/images/artists/noah.jpg', pixiv: 'https://www.pixiv.net/users/87145717', works: ['/my-navigation-site/images/artworks/noah/1.jpg','/my-navigation-site/images/artworks/noah/2.jpg','/my-navigation-site/images/artworks/noah/3.jpg'] , tags: 'VTuber, 少女, 可爱, 天使, 幻想, 色彩, 治愈'},
    { name: '梅原生（せい）', avatar: '/my-navigation-site/images/artists/umehara_sei.jpg', pixiv: 'https://www.pixiv.net/users/3906246', works: ['/my-navigation-site/images/artworks/umehara_sei/1.jpg','/my-navigation-site/images/artworks/umehara_sei/2.jpg','/my-navigation-site/images/artworks/umehara_sei/3.jpg'] , tags: '古风, 少女, 中华, 原创, 兽耳, 优雅, 清新'},
    { name: 'Sua', avatar: '/my-navigation-site/images/artists/sua.jpg', pixiv: 'https://www.pixiv.net/users/9999743', works: ['/my-navigation-site/images/artworks/sua/1.jpg','/my-navigation-site/images/artworks/sua/2.jpg','/my-navigation-site/images/artworks/sua/3.jpg'] , tags: '少女, 鸣潮, 明日方舟, 原创, 色彩, 华丽, 角色'},
    { name: '夢ノ内', avatar: '/my-navigation-site/images/artists/yume_no_uchi.jpg', pixiv: 'https://www.pixiv.net/users/229788', works: ['/my-navigation-site/images/artworks/yume_no_uchi/1.jpg','/my-navigation-site/images/artworks/yume_no_uchi/2.jpg','/my-navigation-site/images/artworks/yume_no_uchi/3.jpg'] , tags: '少女, VOCALOID, 幻想, 帅气, 动态, 色彩, 华丽'},
    { name: '上倉エク', avatar: '/my-navigation-site/images/artists/uekura_eku.jpg', pixiv: 'https://www.pixiv.net/users/299299', works: ['/my-navigation-site/images/artworks/uekura_eku/1.jpg','/my-navigation-site/images/artworks/uekura_eku/2.jpg','/my-navigation-site/images/artworks/uekura_eku/3.jpg'] , tags: '少女, 可爱, 天使, 双马尾, 色彩, 梦幻, 治愈'},
  ];

const navItems = [
  { id: 'home', label: 'Nexus', page: 'home', keywords: '首页 主页 home' },
  { id: 'web', label: '网页', page: 'web', keywords: 'web 网站' },
  { id: 'software', label: '软件', page: 'software', keywords: 'software 应用 程序' },
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
