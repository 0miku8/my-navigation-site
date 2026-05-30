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
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const isDark = ref(true);
const currentPage = ref('home');
const searchQuery = ref('');

const navItems = [
  { id: 'home', label: 'Nexus', page: 'home', keywords: '首页 主页 home' },
  { id: 'web', label: '网页', page: 'web', keywords: 'web 网站' },
  { id: 'software', label: '软件', page: 'software', keywords: 'software 应用 程序' },
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