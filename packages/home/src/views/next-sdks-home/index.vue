<script setup>
// 导入图片资源
import heroBgWhite from "@/assets/images/home/tinyvue-home/web/banner-bg.svg";
import heroBgPc from "@/assets/images/home/tinyvue-home/web/banner-img.svg";
import nextsdkMcpProtocol from "@/assets/images/home/nextsdk_mcp_protocol.png";
import nextsdkRemoter from "@/assets/images/home/nextsdk_remoter.png";
import { ref, computed, onMounted, onUnmounted, nextTick } from "vue";
import nextSdkMd from "./next-sdk.md?raw";
import StepItem from "./components/StepItem.vue";

// 计算步骤总数（用于生成步骤 ID 和循环）
const getStepsCount = () => {
  const parts = nextSdkMd.split(/^### /m).filter((s) => s.trim());
  return parts.length;
};

const totalSteps = getStepsCount();

// 生成步骤数组（仅用于 ID 和索引）
const steps = ref(
  Array.from({ length: totalSteps }, (_, index) => ({
    id: `step-${index + 1}`,
    index,
  }))
);

// 当前激活的步骤索引
const activeStepIndex = ref(0);

// 滚动到指定步骤
const scrollToStep = (index) => {
  if (index < 0 || index >= steps.value.length) return;
  const step = steps.value[index];
  if (step) {
    const element = document.getElementById(step.id);
    if (element) {
      const offsetTop = element.offsetTop - 120; // 留出顶部空间
      window.scrollTo({
        top: offsetTop,
        behavior: "smooth",
      });
      activeStepIndex.value = index;
    }
  }
};

// 使用 IntersectionObserver 监听步骤元素的可见性
let stepObserver = null;

const initStepObserver = () => {
  if (typeof window === "undefined" || typeof document === "undefined") return;
  if (!("IntersectionObserver" in window)) return;

  // 清除旧的观察器
  if (stepObserver) {
    stepObserver.disconnect();
  }

  // 创建观察器，检测步骤元素在视口中的可见性
  stepObserver = new IntersectionObserver(
    (entries) => {
      const triggerOffset = 200; // 触发偏移量
      let currentActive = 0;
      let maxVisibleRatio = 0;

      // 遍历所有步骤，找到在视口中可见度最高的步骤
      for (let i = 0; i < steps.value.length; i++) {
        const step = steps.value[i];
        const element = document.getElementById(step.id);
        if (!element) continue;

        const rect = element.getBoundingClientRect();
        const elementTop = rect.top;
        const elementBottom = rect.bottom;
        const elementHeight = rect.height;
        const viewportHeight = window.innerHeight;

        // 计算元素在视口中的可见部分
        const visibleTop = Math.max(0, elementTop);
        const visibleBottom = Math.min(viewportHeight, elementBottom);
        const visibleHeight = Math.max(0, visibleBottom - visibleTop);
        const visibleRatio = elementHeight > 0 ? visibleHeight / elementHeight : 0;

        // 如果元素的顶部在触发偏移范围内，且元素在视口中可见
        if (elementTop <= triggerOffset && elementBottom > 0) {
          // 如果这个元素可见度更高，选择它
          if (visibleRatio > maxVisibleRatio) {
            maxVisibleRatio = visibleRatio;
            currentActive = i;
          }
        }
      }

      // 如果没找到，从后往前查找最后一个在视口上方的步骤
      if (maxVisibleRatio === 0) {
        for (let i = steps.value.length - 1; i >= 0; i--) {
          const step = steps.value[i];
          const element = document.getElementById(step.id);
          if (element) {
            const rect = element.getBoundingClientRect();
            if (rect.top <= triggerOffset) {
              currentActive = i;
              break;
            }
          }
        }
      }

      if (activeStepIndex.value !== currentActive) {
        activeStepIndex.value = currentActive;
      }
    },
    {
      root: null, // 使用视口作为根
      rootMargin: "0px", // 不使用 rootMargin，手动计算更精确
      threshold: [0, 0.1, 0.25, 0.5, 0.75, 1], // 多个阈值，更精确地检测
    }
  );

  // 观察所有步骤元素
  nextTick(() => {
    steps.value.forEach((step) => {
      const element = document.getElementById(step.id);
      if (element) {
        stepObserver.observe(element);
      }
    });
  });
};

// 响应式屏幕尺寸判断
const windowWidth = ref(typeof window !== "undefined" ? window.innerWidth : 1920);

// 屏幕尺寸断点
const BREAKPOINTS = {
  mobile: 768, // 小屏幕（手机）
};

// 更新窗口宽度
const updateWindowWidth = () => {
  if (typeof window !== "undefined") {
    windowWidth.value = window.innerWidth;
  }
};

// 计算属性：判断屏幕尺寸
const isMobile = computed(() => windowWidth.value < BREAKPOINTS.mobile);

// 进入视口触发 fade-in-up 动效
let fadeObserver;

const initFadeInUp = async () => {
  if (typeof window === "undefined" || typeof document === "undefined") return;
  if (!("IntersectionObserver" in window)) return;

  await nextTick();

  const targets = Array.from(document.querySelectorAll(".fade-in-up"));
  if (!targets.length) return;

  fadeObserver?.disconnect();
  fadeObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("is-visible");
        } else if (entry.intersectionRatio === 0) {
          entry.target.classList.remove("is-visible");
        }
      });
    },
    {
      threshold: 0.1, // 元素出现 10% 就触发
      rootMargin: "0px 0px -50px 0px", // 提前 50px 触发，让动效更自然
    }
  );

  targets.forEach((el) => {
    if (el) fadeObserver.observe(el);
  });
};

// 生命周期：监听窗口大小变化
onMounted(async () => {
  if (typeof window !== "undefined") {
    windowWidth.value = window.innerWidth;
    window.addEventListener("resize", updateWindowWidth);
  }
  initFadeInUp();
  // 延迟初始化，确保所有动态内容都已渲染
  await nextTick();
  setTimeout(() => {
    initFadeInUp();
    // 初始化步骤观察器（确保 DOM 已渲染）
    initStepObserver();
  }, 800);
});

onUnmounted(() => {
  if (typeof window !== "undefined") {
    window.removeEventListener("resize", updateWindowWidth);
  }
  if (stepObserver) {
    stepObserver.disconnect();
    stepObserver = null;
  }
  fadeObserver?.disconnect();
  fadeObserver = undefined;
});
</script>

<template>
  <div class="container">
    <!-- Hero Section: 头部布局与 tiny-vue 一致 -->
    <div class="hero section" :style="{ backgroundImage: `url(${heroBgWhite})` }">
      <div class="hero-content">
        <h1 class="title pad-b40">NEXT-SDKs <br />前端智能应用开发工具包</h1>
        <p class="subtitle pad-b40">让你的前端应用变成智能应用</p>
        <p class="description pad-b40">
          只需四步，即可接入 AI 能力，让应用智能化开发更简单高效
        </p>
        <div class="cta-group">
          <a
            href="https://docs.opentiny.design/next-sdk/guide/"
            target="_blank"
            class="btn primary"
            >快速开始</a
          >
          <a
            href="https://docs.opentiny.design/next-sdk/guide/api-client.html"
            target="_blank"
            class="btn secondary"
            >API 文档</a
          >
        </div>
      </div>
      <div class="hero-img fade-in-up">
        <img :src="heroBgPc" alt="NEXT-SDKs" />
      </div>
    </div>

    <!-- Feature 1: 安装步骤 -->
    <section class="feature-section section bg-tech-1 pad-t40 content-around">
      <div class="feature-header pad-t40 fade-in-up">
        <h2 class="title feature-title">轻松 4 步 让应用智能化</h2>
        <p class="description text-center">
          使用 OpenTiny NEXT-NEXT-SDKs，只需要以下四步，就可以把你的前端应用变成智能应用。
        </p>
      </div>
      <div class="steps-wrapper mar-t40">
        <StepItem
          v-for="(step, index) in steps"
          :key="index"
          :markdown-content="nextSdkMd"
          :step-index="index"
          :step-id="step.id"
          :total-steps="totalSteps"
          :is-active="activeStepIndex === index"
        />
      </div>
      <div class="step-link">
        <a
          href="https://docs.opentiny.design/next-sdk/guide/"
          target="_blank"
          class="btn secondary"
          >阅读使用文档</a
        >
      </div>
    </section>

    <!-- Feature 2: MCP 协议 - 无标题，左右布局，左侧文字右侧图片 -->
    <section
      class="feature-section section bg-tech-2 bg-color-1"
      :style="{ backgroundImage: `url(${heroBgWhite})` }"
    >
      <div class="feature-content pad-t40 fade-in-up">
        <div class="feature-text">
          <h3 class="title">基于 MCP 协议</h3>
          <p class="description">
            支持 WebMcpServer 和 WebMcpClient 双向通信。<br />
            可被各类 MCP Host 操控，实现 AI 与应用的深度集成。<br />
            支持工具注册、资源管理和提示词模板。
          </p>

          <a
            href="https://docs.opentiny.design/next-sdk/guide/api-client.html"
            target="_blank"
            class="btn secondary"
            >了解详情</a
          >
        </div>
        <div class="feature-visual">
          <img :src="nextsdkMcpProtocol" alt="MCP 协议" class="floating-img" />
        </div>
      </div>
    </section>

    <!-- Feature 3: TinyRemoter  遥控器 - 左侧图片右侧文字 -->
    <section
      class="feature-section section bg-tech-1 bg-color-1"
      :style="{ backgroundImage: `url(${heroBgWhite})` }"
    >
      <div class="feature-content pad-t40 fade-in-up">
        <div class="feature-visual">
          <img :src="nextsdkRemoter" alt="TinyRemoter  遥控器" class="floating-img" />
        </div>
        <div class="feature-text">
          <h3 class="title">TinyRemoter 遥控器</h3>
          <p class="description">
            提供网页版 AI 对话框，支持 PC 和移动端。<br />
            通过对话方式让 AI 代替你操作前端应用。<br />
            手机扫码即可远程控制，提升任务完成效率。
          </p>
          <a
            href="https://docs.opentiny.design/next-sdk/guide/tiny-robot-remoter.html"
            target="_blank"
            class="btn secondary"
            >了解详情</a
          >
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped lang="less">
.container {
  width: 100%;
  overflow-x: hidden;
  background-color: var(--bg-color);
}

.step-link {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-bottom: 90px;
  padding-top: 20px;
}
.pad-b40 {
  padding-bottom: 40px !important;
}

.mar-t40 {
  margin-top: 40px !important;
}

.pad-t40 {
  padding-top: 40px !important;
}

.section {
  min-height: 100vh;
  width: 100%;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* Backgrounds */
.hero {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.content-around {
  justify-content: space-around;
}

.bg-color-1 {
  background: #f7fbfe;
}

.bg-tech-1 {
  background-size: cover;
  background-position: center;
}

.bg-tech-2 {
  background-size: cover;
  background-position: center;
}

/* Hero Content */
.hero-content {
  position: relative;
  z-index: 1;
  max-width: 1400px;
  padding: 20px 0 0 40px;
  padding-top: 0;
  animation: fadeInUp 1s ease-out;
}

.hero-img {
  position: relative;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;

  img {
    width: 90%;
    max-width: 1000px;
    filter: drop-shadow(0 20px 40px rgba(0, 0, 0, 0.15));
    border-radius: 20px;
  }
}

.title {
  font-size: 50px;
  font-weight: 700;
  color: #191919;
  letter-spacing: 5px;
  line-height: 1.2;
  padding-bottom: 20px;
}

.subtitle {
  font-size: 44px;
  padding-bottom: 20px;
  line-height: 1.5;
  background: linear-gradient(90deg, #cb43a8 10%, #2c5fef 50%);
  -webkit-background-clip: text;
  background-clip: text;
  letter-spacing: 5px;
  -webkit-text-fill-color: transparent;
  font-weight: 700;
}

.description {
  font-size: 20px;
  padding-bottom: 30px;
  line-height: 2;
  color: #a0a0a0;
  letter-spacing: 3px;
  font-weight: 400;
}

.cta-group {
  display: flex;
  gap: 30px;
}

.btn {
  padding: 16px 48px;
  border-radius: 30px;
  font-size: 19px;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  text-transform: uppercase;
  letter-spacing: 1px;
  position: relative;
  overflow: hidden;
}

.btn.primary {
  background: #191919;
  color: white;
  border: none;
  box-shadow: 0 10px 25px rgba(94, 124, 226, 0.3);
}

.btn.primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 15px 35px rgba(94, 124, 226, 0.5);
}

.btn.secondary {
  background: rgba(255, 255, 255, 0.8);
  color: #191919;
  border: 1px solid #191919;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
}

.btn.secondary:hover {
  background: white;
  transform: translateY(-3px);
  border-color: #191919;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}

/* Feature Sections */
.feature-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 1400px;
  width: 100%;
}

.feature-title {
  font-size: 36px;
}

.feature-content {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: space-around;
  max-width: 1400px;
  width: 100%;
  height: 100%;
}

.max-w1100 {
  max-width: 1100px;
}

.text-center {
  text-align: center;
}

.title-logo {
  display: flex;
  align-items: center;
  gap: 8px;
  padding-bottom: 20px;
}

.feature-sub-title {
  font-size: 20px;
  letter-spacing: 2px;
  padding-bottom: 0;
}

.feature-text {
  color: var(--text-primary);

  .title {
    font-size: 40px;
  }
  .description {
    font-size: 20px;
    letter-spacing: 2px;
    color: #808080;
    letter-spacing: 2px;
    color: #808080;
    padding-bottom: 70px;
    padding-top: 40px;
  }
  .btn {
    background: transparent;
  }
}

.feature-visual {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Steps Wrapper */
.steps-wrapper {
  display: flex;
  width: 100%;
  max-width: 1400px;
  position: relative;
  flex-direction: column;
  border-left: 1px solid #e1e8ed;
}

.floating-img {
  width: 90%;
  max-width: 800px;
}

.fade-in-up {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.fade-in-up.is-visible {
  opacity: 1;
  transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
  .fade-in-up {
    opacity: 1 !important;
    transform: none !important;
    transition: none !important;
  }
  .floating-img {
    animation: none !important;
  }
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-25px);
  }
  100% {
    transform: translateY(0px);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce {
  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translate(-50%, 0);
  }
  40% {
    transform: translate(-50%, -10px);
  }
  60% {
    transform: translate(-50%, -5px);
  }
}

/* ==================== 响应式适配 ==================== */

/* 中等屏幕 (1024px - 1440px) - 小桌面 */
@media (max-width: 1440px) {
  * {
    zoom: 0.9;
  }
}

/* 平板横屏 (1024px - 1440px) */
@media (max-width: 1024px) {
  .hero {
    padding: 40px 30px;
  }

  .hero-img {
    width: 100%;
    margin-top: 40px;
    img {
      width: 100%;
    }
  }

  .title {
    font-size: 24px;
    letter-spacing: 3px;
  }

  .subtitle {
    font-size: 32px;
    letter-spacing: 3px;
  }

  .description {
    font-size: 16px;
  }

  .feature-content {
    flex-direction: row;
    padding: 0 30px;
    gap: 25px;
  }

  .max-w1100 {
    max-width: 100%;
  }

  .feature-text {
    padding: 0;
    margin-bottom: 30px;

    .title {
      font-size: 32px;
    }

    .description {
      font-size: 16px;
      padding-bottom: 40px;
      padding-top: 20px;
    }
  }

  .feature-visual {
    justify-content: center;
  }

  .floating-img {
    max-width: 800px;
  }

  .feature-title {
    font-size: 30px;
  }

  .steps-container {
    max-width: 100%;
  }
}
/* 平板竖屏 / 大手机 (768px - 1024px) */
@media (max-width: 768px) {
  * {
    zoom: 1;
  }
  .section {
    min-height: auto;
    padding: 60px 20px;
  }

  .hero {
    padding: 30px 20px;
    flex-direction: column;
    text-align: center;
    justify-content: center;
  }

  .hero-content {
    padding: 20px;
  }

  .title {
    font-size: 32px;
    letter-spacing: 2px;
    padding-bottom: 15px;
  }

  .subtitle {
    font-size: 26px;
    letter-spacing: 2px;
    padding-bottom: 15px;
  }

  .description {
    padding-bottom: 20px;
  }

  .cta-group {
    gap: 15px;
    width: 100%;
    justify-content: center;
  }

  .btn {
    padding: 14px 30px;
    font-size: 14px;
  }

  .feature-header {
    padding-top: 20px;
  }

  .feature-title {
    font-size: 28px;
  }

  .feature-sub-title {
    font-size: 18px;
    padding-bottom: 0px;
  }
  .title-logo {
    justify-content: center;
  }
  .feature-content {
    padding: 0;
    gap: 10px;
    flex-direction: column;
  }

  // TinyRobot 遥控器部分（第三个 section）：小屏幕时文字在上，图片在下
  .feature-section:nth-of-type(3) .feature-content {
    .feature-text {
      order: 1;
    }

    .feature-visual {
      order: 2;
    }
  }

  .feature-text {
    margin-bottom: 20px;
    text-align: center;

    .title {
      font-size: 28px;
    }

    .description {
      font-size: 16px;
      padding-bottom: 30px;
      padding-top: 20px;
    }
  }

  .feature-visual {
    justify-content: center;
  }

  .floating-img {
    max-width: 100%;
  }

  .pad-b40 {
    padding-bottom: 30px !important;
  }

  .steps-container {
    padding: 0 10px;
    max-width: 100%;
  }
}

/* 小手机 (< 480px) */
@media (max-width: 480px) {
  .section {
    padding: 40px 15px;
  }

  .hero {
    padding: 20px 15px;
  }

  .title {
    font-size: 28px;
    letter-spacing: 1px;
  }

  .subtitle {
    font-size: 22px;
    letter-spacing: 1px;
  }

  .feature-title {
    font-size: 24px;
  }

  .feature-sub-title {
    font-size: 16px;
  }

  .btn {
    padding: 12px 25px;
    font-size: 12px;
  }

  .feature-content {
    padding: 0 15px;
  }

  .feature-text {
    .title {
      font-size: 24px;
    }

    .description {
      font-size: 14px;
      padding-bottom: 20px;
      padding-top: 15px;
    }
  }
}
</style>
