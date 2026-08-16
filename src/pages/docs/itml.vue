<template>
  <div>
    <!-- 页面标题 -->
    <div class="mb-8">
      <div class="flex items-center space-x-4 mb-4">
        <img src="https://itml.shiftcore.top/icon.png" alt="Immortal-TUI-MEFrp-Launcher"
          class="w-12 h-12 rounded-lg" onerror="if(!this.dataset.fallback){this.dataset.fallback='1';this.src='https://itml.shiftcore.top/icon.png'}else{this.style.display='none'}" />
        <div>
          <h1 class="text-3xl font-bold text-white">Immortal-TUI-MEFrp-Launcher</h1>
          <p class="text-gray-400">在 SSH 和真实命令行环境下使用的 ME Frp 图形化终端管理程序</p>
        </div>
      </div>

      <div class="flex flex-wrap gap-2">
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-primary-500/20 text-primary-400">Python</span>
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-green-500/20 text-green-400">Textual</span>
        <span class="px-2 py-1 text-xs font-medium rounded-full bg-blue-500/20 text-blue-400">TUI</span>
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
          <strong class="text-primary-400">Immortal-TUI-MEFrp-Launcher</strong>（简称 <strong class="text-primary-400">ITML</strong>）是由
          <strong class="text-primary-400">zqamemz</strong> 开发的 ME Frp 图形化终端管理程序，
          基于 Textual 框架构建，无需 WebUI 即可在终端中完成从登录到守护的一站式隧道管理。
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
                <span>密码 / Token 登录，自动完成人机验证</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>隧道管理：列表、创建、启用/禁用、删除，创建时实时展示节点负载与带宽</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Systemd 守护：隧道一键安装为系统服务，开机自启、进程守护</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>内置 mefrpc：首次启动自动安装，无需单独下载 frpc</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-check-circle" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>每日签到与权益抽取，均支持人机验证</span>
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
                <span>Python 3.8 或更高版本</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-terminal" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Textual &ge; 1.0.0、Requests &ge; 2.25.0、PyYAML &ge; 5.1</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-server" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Linux：Debian、Ubuntu 等主流发行版（SSH 环境亦可）</span>
              </li>
              <li class="flex items-start space-x-2">
                <UIcon name="i-lucide-shield-check" class="size-4 text-primary-400 mt-0.5 flex-shrink-0" />
                <span>Systemd 守护功能需 root / sudo 权限</span>
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
            :alt="previewImages[currentImageIndex].alt + ' - Immortal-TUI-MEFrp-Launcher 界面截图'"
            class="max-w-full max-h-full object-contain cursor-pointer" loading="lazy"
            @click="openImageModal({ src: currentPreviewSrc, alt: previewImages[currentImageIndex].alt })" />

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

    <!-- 下载安装 -->
    <div ref="downloadRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': downloadVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">下载安装</h2>
      </div>

      <div class="p-6 space-y-6">
        <div class="text-center">
          <UButton size="lg" color="primary" to="https://itml.shiftcore.top/docs.html" target="_blank"
            class="btn-glow cursor-pointer">
            <UIcon name="i-lucide-download" class="size-5 mr-2" />
            官网文档下载
          </UButton>
        </div>

        <div class="p-4 rounded-lg bg-yellow-500/10 border border-yellow-500/20">
          <div class="flex items-start space-x-3">
            <UIcon name="i-lucide-alert-triangle" class="size-5 text-yellow-400 mt-0.5 flex-shrink-0" />
            <div>
              <h4 class="font-semibold text-yellow-400">系统要求</h4>
              <p class="text-gray-300 text-sm mt-1">需要 Python 3.8 或更高版本，建议在 Linux 系统（Debian / Ubuntu 等）下使用。可以在终端输入 <code class="text-primary-400">python3 --version</code> 检查。</p>
            </div>
          </div>
        </div>

        <div class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-smooth">
          <h3 class="text-lg font-semibold text-white mb-3">安装步骤</h3>
          <ol class="space-y-4 text-gray-300">
            <li class="flex items-start space-x-3">
              <span class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">1</span>
              <div class="flex-1 min-w-0">
                <p><strong class="text-white">一键部署</strong>（仅支持 Debian / Ubuntu 系列系统）：</p>
                <div class="bg-gray-900 rounded-lg p-3 mt-2 overflow-x-auto">
                  <pre class="text-sm text-gray-300"><code>curl -fsSL https://itml.shiftcore.top/onekeyinstall.sh | bash</code></pre>
                </div>
              </div>
            </li>
            <li class="flex items-start space-x-3">
              <span class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">2</span>
              <div class="flex-1 min-w-0">
                <p><strong class="text-white">从源码安装</strong>：</p>
                <div class="bg-gray-900 rounded-lg p-3 mt-2 overflow-x-auto">
                  <pre class="text-sm text-gray-300"><code>git clone https://github.com/zqamemz/Immortal-TUI-MEFrp-Launcher.git /opt/itml
cd /opt/itml
pip install textual requests pyyaml
pip install -e .</code></pre>
                </div>
              </div>
            </li>
            <li class="flex items-start space-x-3">
              <span class="flex-shrink-0 w-6 h-6 bg-primary-500/20 text-primary-400 rounded-full flex items-center justify-center text-sm font-bold">3</span>
              <div class="flex-1 min-w-0">
                <p><strong class="text-white">pip 安装</strong>（发布后可用）：</p>
                <div class="bg-gray-900 rounded-lg p-3 mt-2 overflow-x-auto">
                  <pre class="text-sm text-gray-300"><code>pip install itml</code></pre>
                </div>
              </div>
            </li>
          </ol>
        </div>
      </div>
    </div>

    <!-- 使用指南 -->
    <div ref="guideRef" class="mb-8 glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': guideVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">使用指南</h2>
      </div>

      <div class="p-6 space-y-6">
        <div class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-smooth">
          <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
            <UIcon name="i-lucide-terminal" class="size-5 text-primary-400" />
            启动程序
          </h3>
          <div class="bg-gray-900 rounded-lg p-3 overflow-x-auto">
            <pre class="text-sm text-gray-300"><code>sml

# 或使用 Python 模块方式启动
python -m sml</code></pre>
          </div>
        </div>

        <div class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-smooth">
          <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
            <UIcon name="i-lucide-layout-dashboard" class="size-5 text-primary-400" />
            首次使用流程
          </h3>
          <p class="text-gray-300 text-sm">
            登录（账号密码或访问 Token）→ 在主菜单「设置」中配置 frpc 路径 → 进入「隧道列表」查看已有隧道或「新建隧道」→ 在隧道详情中安装 Systemd 服务实现进程守护与开机自启。
          </p>
          <div class="text-gray-400 text-xs mt-2">
            提示：配置保存在 <code class="text-primary-400">~/.config/itml/config.json</code>，包含 token、username、frpc_path 等字段。
          </div>
        </div>

        <div class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-smooth">
          <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
            <UIcon name="i-lucide-server" class="size-5 text-primary-400" />
            Systemd 守护管理
          </h3>
          <p class="text-gray-300 text-sm mb-3">隧道安装为系统服务后，可通过 systemd 手动管理：</p>
          <div class="bg-gray-900 rounded-lg p-3 overflow-x-auto">
            <pre class="text-sm text-gray-300"><code># 查看所有 ITML 隧道服务
systemctl list-units --type=service | grep itml-tunnel

# 查看单个隧道状态
systemctl status itml-tunnel-10086

# 查看隧道日志
journalctl -u itml-tunnel-10086 -f</code></pre>
          </div>
        </div>

        <div class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-smooth">
          <h3 class="text-lg font-semibold text-white mb-2 flex items-center gap-2">
            <UIcon name="i-lucide-log-out" class="size-5 text-primary-400" />
            退出方式
          </h3>
          <p class="text-gray-300 text-sm">
            按 <code class="text-primary-400">Ctrl+Q</code> 弹出退出确认菜单，可选择「保持隧道退出」（隧道继续运行，仅关闭 UI）或「关闭隧道退出」（停止所有 systemd 隧道服务后退出）。
          </p>
        </div>
      </div>
    </div>

    <!-- 相关链接 -->
    <div ref="linksRef" class="glass-card rounded-xl overflow-hidden scroll-animate hover-lift"
      :class="{ 'visible': linksVisible }">
      <div class="p-6 border-b border-white/10">
        <h2 class="text-xl font-semibold text-white">相关链接</h2>
      </div>

      <div class="p-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <a href="https://github.com/zqamemz/Immortal-TUI-MEFrp-Launcher" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-github" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">GitHub 仓库</h3>
              <p class="text-sm text-gray-400">查看源代码</p>
            </div>
          </a>

          <a href="https://itml.shiftcore.top" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-globe" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">官网</h3>
              <p class="text-sm text-gray-400">产品介绍与预览</p>
            </div>
          </a>

          <a href="https://itml.shiftcore.top/docs.html" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-book-open" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">文档页</h3>
              <p class="text-sm text-gray-400">安装与使用说明</p>
            </div>
          </a>

          <a href="https://github.com/zqamemz/Immortal-TUI-MEFrp-Launcher/issues" target="_blank"
            class="glass-card p-4 rounded-lg hover-lift flex items-center space-x-3 cursor-pointer">
            <UIcon name="i-lucide-message-square" class="size-6 text-primary-400" />
            <div>
              <h3 class="font-medium text-white">问题反馈</h3>
              <p class="text-sm text-gray-400">提交 Bug 与建议</p>
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

// 滚动动画
const { elementRef: introRef, isVisible: introVisible } = useScrollAnimation()
const { elementRef: previewRef, isVisible: previewVisible } = useScrollAnimation()
const { elementRef: downloadRef, isVisible: downloadVisible } = useScrollAnimation()
const { elementRef: guideRef, isVisible: guideVisible } = useScrollAnimation()
const { elementRef: linksRef, isVisible: linksVisible } = useScrollAnimation()

// 预览图数据
const previewImages = [
  { src: 'https://itml.shiftcore.top/pic1.png', alt: '主界面' },
  { src: 'https://itml.shiftcore.top/pic2.png', alt: '界面截图 2' },
  { src: 'https://itml.shiftcore.top/pic3.png', alt: '界面截图 3' }
]

const currentImageIndex = ref(0)
const showImageModal = ref(false)
const modalImage = ref({ src: '', alt: '' })

// 当前显示的图片 src
const currentPreviewSrc = computed(() => {
  return previewImages[currentImageIndex.value].src
})

const nextImage = () => {
  currentImageIndex.value = (currentImageIndex.value + 1) % previewImages.length
}

const previousImage = () => {
  currentImageIndex.value = currentImageIndex.value === 0 ? previewImages.length - 1 : currentImageIndex.value - 1
}

const openImageModal = (image) => {
  modalImage.value = image
  showImageModal.value = true
}

// 页面元数据
useHead({
  title: 'Immortal-TUI-MEFrp-Launcher 文档',
  link: [
    { rel: 'canonical', href: `${SITE_URL}/docs/itml` }
  ],
  script: [
    {
      type: 'application/ld+json',
      innerHTML: JSON.stringify({
        '@context': 'https://schema.org',
        '@type': 'SoftwareApplication',
        name: 'Immortal-TUI-MEFrp-Launcher',
        applicationCategory: 'NetworkApplication',
        operatingSystem: 'Linux',
        description: '在 SSH 和真实命令行环境下使用的 ME Frp 图形化终端管理程序，基于 Textual 构建，无需 WebUI 即可完成隧道管理',
        author: { '@type': 'Person', name: 'zqamemz' },
        offers: {
          '@type': 'Offer',
          price: '0',
          priceCurrency: 'CNY'
        }
      })
    }
  ]
})

// SEO 优化
useSeoMeta({
  title: `Immortal-TUI-MEFrp-Launcher 文档 | ${SITE_NAME}`,
  ogTitle: `Immortal-TUI-MEFrp-Launcher 文档 - ${SITE_NAME}`,
  description: 'Immortal-TUI-MEFrp-Launcher（ITML）是由 zqamemz 基于 Textual 开发的 ME Frp 图形化终端管理程序，在 SSH 与命令行环境中即可完成隧道管理、Systemd 守护、每日签到等操作。',
  ogDescription: 'Immortal-TUI-MEFrp-Launcher 基于 Textual 的 ME Frp TUI 管理程序，无需 WebUI 即可完成隧道管理',
  ogImage: OG_IMAGE,
  ogUrl: `${SITE_URL}/docs/itml`,
  ogType: 'article',
  twitterCard: 'summary_large_image'
})
</script>
