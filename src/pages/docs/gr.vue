<template>
  <div>
    <!-- 页面标题 -->
    <div class="mb-8">
      <div class="flex items-center space-x-4 mb-4">
        <img :src="resolvedIconSrc" alt="MeFrp-GR-Client" class="w-12 h-12 rounded-lg" @error="onIconError" />
        <div>
          <h1 class="text-3xl font-bold text-white">MeFrp-GR-Client</h1>
          <p class="text-gray-400">基于 Python + Flask 的跨平台 FRP 客户端</p>
        </div>
      </div>

      <div class="flex flex-wrap gap-2">
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-primary-500/20 text-primary-400">Python</span>
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-green-500/20 text-green-400">跨平台</span>
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-blue-500/20 text-blue-400">Flask</span>
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-purple-500/20 text-purple-400">PyQt5</span>
      </div>
    </div>

    <!-- 项目介绍 -->
    <div ref="introRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': introVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">项目介绍</h2>
      </div>

      <div class="p-6 space-y-4 text-gray-300">
        <p>
          <strong class="text-primary-400">MeFrp-GR-Client</strong> 是由
          <strong class="text-primary-400">Guoran_11</strong> 开发的 ME-Frp 第三方客户端，
          使用 Python + Flask 后端配合 PyQt5 WebView 图形界面，支持 Windows、Linux 和 fnOS 多平台，
          自动管理 frpc 生命周期，提供 Token 认证与隧道管理功能，让您轻松使用 ME-Frp 内网穿透服务。
        </p>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <h3 class="text-lg font-semibold text-white mb-3 flex items-center gap-2">
              <UIcon name="i-lucide-sparkles" class="size-5 text-primary-400" />
              主要特性
            </h3>
            <ul class="space-y-2">
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>跨平台支持 Windows、Linux (.deb)、fnOS (NAS)</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>内置 WebView 图形界面，操作直观便捷</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>自动下载与管理 frpc 生命周期</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Token 认证登录，安全可靠</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>隧道一键启停，流量信息实时展示</span>
              </li>
            </ul>
          </div>

          <div>
            <h3 class="text-lg font-semibold text-white mb-3 flex items-center gap-2">
              <UIcon name="i-lucide-monitor" class="size-5 text-primary-400" />
              系统要求
            </h3>
            <ul class="space-y-2">
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-cpu" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Windows 10/11 (x64)</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-monitor" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Linux：Debian/Ubuntu 等 (amd64/arm64/i386)</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-hard-drive" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>fnOS：飞牛 NAS 系统 (Docker 部署)</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-wifi" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>需要网络连接以访问 ME-Frp API</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <!-- 预览图 -->
    <div ref="previewRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': previewVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">预览图</h2>
      </div>

      <div class="p-6 relative rounded-lg overflow-hidden bg-gray-900/50">
        <div class="relative h-96 flex items-center justify-center">
          <img :key="currentImageIndex" :src="currentPreviewSrc"
            :alt="previewImages[currentImageIndex].alt + ' - MeFrp-GR-Client 界面截图'"
            class="max-w-full max-h-full object-contain cursor-pointer" loading="lazy"
            @click="openImageModal({ src: currentPreviewSrc, alt: previewImages[currentImageIndex].alt })"
            @error="onPreviewImageError" />

          <div class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/80 to-transparent p-4">
            <p class="text-white text-center font-medium">{{ previewImages[currentImageIndex].alt }}</p>
          </div>
        </div>

        <button @click="previousImage"
          class="absolute left-4 top-1/2 transform -translate-y-1/2 bg-black/50 hover:bg-black/70 text-white p-2 rounded-full transition-colors"
          :disabled="previewImages.length <= 1">
          <UIcon name="i-lucide-chevron-left" class="size-5" />
        </button>

        <button @click="nextImage"
          class="absolute right-4 top-1/2 transform -translate-y-1/2 bg-black/50 hover:bg-black/70 text-white p-2 rounded-full transition-colors"
          :disabled="previewImages.length <= 1">
          <UIcon name="i-lucide-chevron-right" class="size-5" />
        </button>

        <div class="absolute bottom-16 left-1/2 transform -translate-x-1/2 flex space-x-2">
          <button v-for="(image, index) in previewImages" :key="index" @click="currentImageIndex = index"
            class="w-2 h-2 rounded-full transition-colors"
            :class="index === currentImageIndex ? 'bg-white' : 'bg-white/50'" />
        </div>
      </div>
    </div>

    <!-- 图片放大模态框 -->
    <Teleport to="body">
      <div v-if="showImageModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/80 backdrop-blur-sm"
        @click="showImageModal = false">
        <div class="relative max-w-[90vw] max-h-[90vh]">
          <img :src="modalImage.src" :alt="modalImage.alt" class="max-w-full max-h-[90vh] object-contain rounded-lg" />
          <button @click="showImageModal = false"
            class="absolute top-4 right-4 bg-black/50 hover:bg-black/70 text-white p-2 rounded-full transition-colors">
            <UIcon name="i-lucide-x" class="size-5" />
          </button>
          <p class="text-white text-center mt-4 font-medium">{{ modalImage.alt }}</p>
        </div>
      </div>
    </Teleport>

    <!-- 功能清单 -->
    <div ref="featuresRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': featuresVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">功能清单</h2>
      </div>

      <div class="p-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div class="glass-card rounded-lg p-4 hover-lift overflow-hidden">
            <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
              <UIcon name="i-lucide-key" class="size-5 text-primary-400" />
              Token 认证
            </h3>
            <p class="text-gray-400 text-sm">通过 ME-Frp 官方 API Token 登录，安全可靠，无需输入账号密码</p>
          </div>

          <div class="glass-card rounded-lg p-4 hover-lift overflow-hidden">
            <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
              <UIcon name="i-lucide-download" class="size-5 text-primary-400" />
              自动管理 frpc
            </h3>
            <p class="text-gray-400 text-sm">自动下载对应平台的 frpc 二进制文件，无需手动配置</p>
          </div>

          <div class="glass-card rounded-lg p-4 hover-lift overflow-hidden">
            <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
              <UIcon name="i-lucide-play-circle" class="size-5 text-primary-400" />
              隧道管理
            </h3>
            <p class="text-gray-400 text-sm">一键启停隧道，查看隧道状态与连接信息</p>
          </div>

          <div class="glass-card rounded-lg p-4 hover-lift overflow-hidden">
            <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
              <UIcon name="i-lucide-bar-chart-3" class="size-5 text-primary-400" />
              流量监控
            </h3>
            <p class="text-gray-400 text-sm">实时查看账户流量使用情况与剩余流量</p>
          </div>

          <div class="glass-card rounded-lg p-4 hover-lift overflow-hidden">
            <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
              <UIcon name="i-lucide-layout-dashboard" class="size-5 text-primary-400" />
              图形界面
            </h3>
            <p class="text-gray-400 text-sm">基于 PyQt5 WebView 的现代化图形界面，美观易用</p>
          </div>

          <div class="glass-card rounded-lg p-4 hover-lift overflow-hidden">
            <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
              <UIcon name="i-lucide-laptop" class="size-5 text-primary-400" />
              多平台支持
            </h3>
            <p class="text-gray-400 text-sm">支持 Windows、Linux (.deb) 和 fnOS 飞牛 NAS 多平台部署</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 下载安装 -->
    <div ref="downloadRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': downloadVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">下载安装</h2>
      </div>

      <div class="p-6 space-y-6">
        <div class="text-center">
          <UButton size="lg" color="primary" :href="DOWNLOAD_URL" target="_blank" rel="noopener noreferrer"
            class="btn-glow cursor-pointer">
            <UIcon name="i-lucide-download" class="size-5 mr-2" />
            下载 MeFrp-GR-Client
          </UButton>
        </div>

        <div class="p-4 rounded-lg bg-yellow-500/10 border border-yellow-500/20">
          <div class="flex items-start space-x-3">
            <UIcon name="i-lucide-alert-triangle" class="size-5 text-yellow-400 mt-0.5 flex-shrink-0" />
            <div>
              <h4 class="font-semibold text-yellow-400">系统要求</h4>
              <p class="text-gray-300 text-sm mt-1">Windows 版本已内置 Python 运行环境，无需额外安装。Linux 版本需安装
                <code class="text-primary-400">python3</code>、<code
                  class="text-primary-400">python3-pyqt5.qtwebengine</code> 等依赖。
              </p>
            </div>
          </div>
        </div>

        <div class="p-4 rounded-lg bg-blue-500/10 border border-blue-500/20">
          <div class="flex items-start space-x-3">
            <UIcon name="i-lucide-info" class="size-5 text-blue-400 mt-0.5 flex-shrink-0" />
            <div>
              <h4 class="font-semibold text-blue-400">使用说明</h4>
              <p class="text-gray-300 text-sm mt-1">请使用 ME-Frp 官网获取的 Token 登录。首次启动时程序会自动下载所需的 frpc
                依赖文件。</p>
            </div>
          </div>
        </div>

        <div class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-smooth">
          <h3 class="text-lg font-semibold text-white mb-3">安装步骤</h3>
          <ol class="space-y-2 text-gray-300">
            <li class="flex items-start space-x-3">
              <span
                class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">1</span>
              <span>从下载中心获取对应平台的安装包（Windows / Linux .deb / fnOS .fpk）</span>
            </li>
            <li class="flex items-start space-x-3">
              <span
                class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">2</span>
              <span>Windows：解压后运行 <code class="text-primary-400">MeFrp-GR-Client.exe</code></span>
            </li>
            <li class="flex items-start space-x-3">
              <span
                class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">3</span>
              <span>Linux：安装 .deb 包 <code class="text-primary-400">sudo dpkg -i mefrp-gr-client.deb</code></span>
            </li>
            <li class="flex items-start space-x-3">
              <span
                class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">4</span>
              <span>fnOS：在飞牛应用中心手动安装 <code class="text-primary-400">.fpk</code> 包</span>
            </li>
            <li class="flex items-start space-x-3">
              <span
                class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">5</span>
              <span>在登录页面输入 ME-Frp Token 完成认证</span>
            </li>
            <li class="flex items-start space-x-3">
              <span
                class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">6</span>
              <span>选择隧道并点击启动，即可开始使用</span>
            </li>
          </ol>
        </div>
      </div>
    </div>

    <!-- 更新记录 -->
    <div ref="updateRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': updateVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">更新记录</h2>
      </div>
      <ChangelogList :updates="updates" :loading="loading" :error="error" :page-size="5" />
    </div>

    <!-- 相关链接 -->
    <div ref="linksRef" class="glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': linksVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">相关链接</h2>
      </div>

      <div class="p-6">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <a :href="DOWNLOAD_URL" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-download" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">下载中心</h3>
              <p class="text-sm text-gray-400">获取最新版本</p>
            </div>
          </a>

          <a href="https://www.mefrp.com" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-globe" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">ME-Frp 官网</h3>
              <p class="text-sm text-gray-400">注册账号</p>
            </div>
          </a>

          <a href="https://www.mefrp.com/3rdparty/captcha?client=MeFrpGR" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-key" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">获取 Token</h3>
              <p class="text-sm text-gray-400">API 认证令牌</p>
            </div>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { SITE_URL, SITE_NAME, OG_IMAGE } from "~/data/constants";

// 使用文档布局
definePageMeta({
  layout: 'docs'
})

// ============ 站点常量 ============
// 下载中心：自建 OpenList 网盘
const DOWNLOAD_URL = 'https://openlist.guo-ran.top:5244/MeFrp-GR-Client'

// 页面图标：优先使用站点自带的新版 GR 图标（src/public/icon/），
// 加载失败时回退到图床旧图标，保证任何部署环境下都不会出现空白。
const ICON_SRC = '/icon/gr-logo-128.png'
const ICON_FALLBACK_SRC = 'https://image.mefrp-tpca.yealqp.cn/images/views/icon/gr_icon.webp'

// 滚动动画
const { elementRef: introRef, isVisible: introVisible } = useScrollAnimation()
const { elementRef: previewRef, isVisible: previewVisible } = useScrollAnimation()
const { elementRef: featuresRef, isVisible: featuresVisible } = useScrollAnimation()
const { elementRef: downloadRef, isVisible: downloadVisible } = useScrollAnimation()
const { elementRef: updateRef, isVisible: updateVisible } = useScrollAnimation()
const { elementRef: linksRef, isVisible: linksVisible } = useScrollAnimation()

// ============ 图标容错：webp 加载失败回退 png ============
const iconFailed = ref(false)
const onIconError = () => {
  if (!iconFailed.value) iconFailed.value = true
}
const resolvedIconSrc = computed(() => (iconFailed.value ? ICON_FALLBACK_SRC : ICON_SRC))

// ============ 预览图数据 ============
const previewImages = [
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/01-%E4%B8%BB%E7%95%8C%E9%9D%A2.png?sign=XwFatAx9kcr_kRth0EbEFt1CxpSiVIAXcAgsqW-3qBE=:0', alt: '主界面' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/02-%E7%99%BB%E5%BD%95%E9%A1%B5%E6%A0%87%E6%B3%A8.png?sign=MPogVDc9XHCE8lsrtGVHUB2JN6M88ju0JH2DqZ3dPhs=:0', alt: '登录页面' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/03-Token%E7%99%BB%E5%BD%95%E8%A1%A8%E5%8D%95.png?sign=ew1QZdNiakvqnQltCvJFHCXpeptTvsS1nJZZyDnJ530=:0', alt: 'Token 登录' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/04-%E6%B3%A8%E5%86%8C%E9%A1%B5%E9%9D%A2.png?sign=Zl4cunzQ1WyaUAEWy7zEvXDGvFSR1gv5WlIXmtjnuaA=:0', alt: '注册页面' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/05-%E6%8E%A7%E5%88%B6%E5%8F%B0.png?sign=MhTNXHyaWMRA_jlOs-BJnrE_Jn1aWhzJfzMvORv4vfc=:0', alt: '控制台' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/06-%E9%80%9A%E9%81%93%E7%AE%A1%E7%90%86.png?sign=pCQ92RKTeILQ353zC5pmgJ_IpPzqDt2GGAUpsBwjs4k=:0', alt: '隧道管理' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/07-%E8%8A%82%E7%82%B9%E7%9B%91%E6%8E%A7.png?sign=0FHGiAhitVIMMd6UXPS2m9W22xMlFL7cqor9K4IlhBs=:0', alt: '节点监控' },
  { src: 'https://openlist.guo-ran.top:5244/p/MeFrp-GR-Client/icon/08-%E7%94%A8%E6%88%B7%E4%B8%AD%E5%BF%83.png?sign=VD0BVvyruofKN5AXvPY87b4XryB9IShLWasXgQ5ON8Y=:0', alt: '用户中心' },
]

const currentImageIndex = ref(0)
const showImageModal = ref(false)
const modalImage = ref({ src: '', alt: '' })

// 每张图片的回退状态：false = 使用 CDN，true = 已回退到 OSS
const fallbackState = ref(previewImages.map(() => false))

// 当前显示的图片 src（根据回退状态决定用 CDN 还是 OSS）
const currentPreviewSrc = computed(() => {
  const img = previewImages[currentImageIndex.value]
  if (fallbackState.value[currentImageIndex.value]) {
    return img.src.replace('image.mefrp-tpca.yealqp.cn', 'oss.cf.xiaofanshop.cn/tpcaw')
  }
  return img.src
})

// 预览图加载失败时的处理
const onPreviewImageError = () => {
  const idx = currentImageIndex.value
  if (!fallbackState.value[idx]) {
    fallbackState.value[idx] = true
  }
}

const nextImage = () => {
  currentImageIndex.value = (currentImageIndex.value + 1) % previewImages.length
}

const previousImage = () => {
  currentImageIndex.value = currentImageIndex.value === 0 ? previewImages.length - 1 : currentImageIndex.value - 1
}

const openImageModal = (image: { src: string; alt: string }) => {
  modalImage.value = image
  showImageModal.value = true
}

// ============ 更新日志 ============
const { updates, loading, error, fetchChangelog } = useProductChangelog('gr')

// 组件挂载时初始化
onMounted(() => {
  fetchChangelog()
})

// ============ 页面元数据 ============
useHead({
  title: 'MeFrp-GR-Client 文档',
  link: [
    { rel: 'canonical', href: `${SITE_URL}/docs/gr` }
  ],
  script: [
    {
      type: 'application/ld+json',
      innerHTML: JSON.stringify({
        '@context': 'https://schema.org',
        '@type': 'SoftwareApplication',
        name: 'MeFrp-GR-Client',
        applicationCategory: 'NetworkApplication',
        operatingSystem: 'Windows, Linux, fnOS',
        description: '基于 Python + Flask 的跨平台 FRP 客户端，支持 Windows、Linux 和 fnOS 多平台',
        author: { '@type': 'Person', name: 'Guoran_11' },
        offers: {
          '@type': 'Offer',
          price: '0',
          priceCurrency: 'CNY'
        }
      })
    }
  ]
})

// ============ SEO 优化 ============
useSeoMeta({
  title: `MeFrp-GR-Client 文档 | ${SITE_NAME}`,
  ogTitle: `MeFrp-GR-Client 文档 - ${SITE_NAME}`,
  description: 'MeFrp-GR-Client 是由 Guoran_11 基于 Python + Flask 开发的跨平台 FRP 客户端，支持 Windows、Linux 和 fnOS，内置 WebView 图形界面，自动管理 frpc 生命周期。',
  ogDescription: 'MeFrp-GR-Client 跨平台 FRP 客户端，支持 Windows、Linux 和 fnOS',
  ogImage: OG_IMAGE,
  ogUrl: `${SITE_URL}/docs/gr`,
  ogType: 'article',
  twitterCard: 'summary_large_image'
})
</script>
