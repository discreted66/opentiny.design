<script setup>
// 导入图片资源
import heroBgWhite from '@/assets/images/home/hero_bg_white.png'
import featureCrossPlatform from '@/assets/images/home/feature_cross_platform.png'
import illustrationRichComponents from '@/assets/images/home/illustration_rich_components.png'
import illustrationUniqueComponents from '@/assets/images/home/illustration_unique_components.png'
import tinyvueThemeCustomization from '@/assets/images/home/tinyvue_theme_customization.png'
import featureBgWhite1 from '@/assets/images/home/feature_bg_white_1.png'
import featureBgWhite2 from '@/assets/images/home/feature_bg_white_2.png'
import mcpBgWhite from '@/assets/images/home/mcp_bg_white.png'
import iconWindow from '@/assets/tech/icon-window.svg'
import iconPhone from '@/assets/tech/icon-phone.svg'
import iconShare from '@/assets/tech/icon-share.svg'
import { ref, reactive } from 'vue'
import {
  TinyButton,
  TinySlider,
  TinySteps,
  TinyAlert,
  TinyAnchor,
  TinyCheckbox,
  TinyDatePanel,
  TinyInput,
  TinyTreeMenu,
  TinySearch,
  TinyRow,
  TinyLink
} from '@opentiny/vue'

import { IconSearch } from '@opentiny/vue-icon'
import {
  iconDeleteL,
  iconYes,
  iconEditor,
  iconMail,
  iconStarO,
  iconSearch,
  iconFilletExternalLink
} from '@opentiny/vue-icon'

const TinyIconFilletExternalLink = iconFilletExternalLink()
const TinyIconDeleteL = iconDeleteL()
const TinyIconYes = iconYes()
const TinyIconEditor = iconEditor()
const TinyIconMail = iconMail()
const TinyIconStarO = iconStarO()
const TinyIconSearch = iconSearch()

let expandeArr = reactive([301])
const highlight = ref(301)
const treeData = reactive([
  {
    id: 300,
    label: 'TinyVue',
    children: [
      {
        id: 301,
        label: 'Button'
      },
      { id: 302, label: 'ButtonGroup' },
      { id: 303, label: 'Container' },
      { id: 304, label: 'Icon' },
      { id: 305, label: 'Layout' },
      { id: 306, label: 'Link' },
      { id: 307, label: 'Divider' },
      { id: 308, label: 'VirtualScrollBox' }
    ]
  }
])

const slidValue = ref(100)
const stepsData = reactive([
  {
    name: '1',
    status: 'done'
  },
  { name: '2', status: 'doing' },
  { name: '3' },
  { name: '4' }
])
const active = ref(1)

const anchorData = reactive([
  {
    key: 'overview',
    title: '概览'
  },
  {
    key: 'pod-list',
    title: 'Pod列表'
  },
  {
    key: 'lod-list',
    title: 'Lod列表'
  },
  {
    key: 'access-config',
    title: '访问配置'
  },
  {
    key: 'config-pod',
    title: '配置Pod'
  },
  {
    key: 'event',
    title: '事件'
  },
  {
    key: 'history-version',
    title: '历史版本'
  }
])

// 跨端跨框架特性数据
const crossPlatformFeatures = reactive([
  {
    title: '一套代码同时支持PC和移动端',
    description: '我们支持PC和移动端，同一个组件在不同终端表现不同在多端场景下组件的使用方式完全相同。',
    icon: '/@fs/D:/opentiny.design/packages/common/src/assets/appIcon/tiny-vue.svg'
  },
  {
    title: '一套代码同时支持Vue 2 / Vue 3',
    description:
      '针对Vue2 和 Vue3 实现了相应的版本适配器，抹平Vue2 和 Vue3 的差异，实现了一套代码同时支持Vue2 和 Vue3。',
    icon: '/@fs/D:/opentiny.design/packages/common/src/assets/appIcon/tiny-vue.svg'
  }
])

// 配置式组件架构数据
const configArchitecture = reactive([
  {
    title: '无渲染层',
    description:
      '包含纯业务逻辑，无 UI 依赖，例如，Button 组件的 handleClick 函数管理点击事件、计时器逻辑和状态变更，不涉及任何 DOM 操作。',
    backgroundColor: 'red'
  },
  {
    title: '适配层',
    description:
      '通过抽象框架特定 API 提供 Vue 2/3 兼容性，这使得形同的无渲染逻辑可在不同 Vue 版本上工作，无需重复开发，无需在组件实现中使用条件逻辑。',
    backgroundColor: 'orange'
  },
  {
    title: '展示层',
    description:
      '包含实际的 UI 模板，有独立的 PC 和移动端变体，例如，Bu tton 组件有独立的 PC 和移动优先 Vue 文件，他们消费相同的无渲染逻辑。',
    backgroundColor: 'green'
  }
])

// 智能化生成式 UI 特性数据
const mcpFeatures = reactive([
  {
    title: '属性驱动式组件',
    description: '通过简单的属性配置即可生成复杂的 UI 组件，降低开发门槛',
    iconPath:
      'M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm-5 14H7v-2h7v2zm3-4H7v-2h10v2zm0-4H7V7h10v2z'
  },
  {
    title: 'AI 可识别智能组件',
    description: '组件可被 AI 识别和操作，实现智能化的应用开发体验',
    iconPath:
      'M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z'
  },
  {
    title: '轻量化组件，性能强悍',
    description: '优化组件体积和性能，确保应用运行流畅高效',
    iconPath:
      'M13 2.05v3.03c3.39.49 6 3.39 6 6.92 0 .9-.18 1.75-.48 2.54l2.6 1.53c.56-1.24.88-2.62.88-4.07 0-5.18-3.95-9.45-9-9.95zM12 19c-3.87 0-7-3.13-7-7 0-3.53 2.61-6.43 6-6.92V2.05c-5.06.5-9 4.76-9 9.95 0 5.52 4.47 10 9.99 10 3.31 0 6.24-1.61 8.06-4.09l-2.6-1.53C16.17 17.98 14.21 19 12 19z'
  },
  {
    title: '轻松适配多主题',
    description: '灵活的主题系统，支持快速切换和定制企业品牌风格',
    iconPath:
      'M12 3c-4.97 0-9 4.03-9 9s4.03 9 9 9c.83 0 1.5-.67 1.5-1.5 0-.39-.15-.74-.39-1.01-.23-.26-.38-.61-.38-.99 0-.83.67-1.5 1.5-1.5H16c2.76 0 5-2.24 5-5 0-4.42-4.03-8-9-8zm-5.5 9c-.83 0-1.5-.67-1.5-1.5S5.67 9 6.5 9 8 9.67 8 10.5 7.33 12 6.5 12zm3-4C8.67 8 8 7.33 8 6.5S8.67 5 9.5 5s1.5.67 1.5 1.5S10.33 8 9.5 8zm5 0c-.83 0-1.5-.67-1.5-1.5S13.67 5 14.5 5s1.5.67 1.5 1.5S15.33 8 14.5 8zm3 4c-.83 0-1.5-.67-1.5-1.5S16.67 9 17.5 9s1.5.67 1.5 1.5-.67 1.5-1.5 1.5z'
  }
])
</script>

<template>
  <div class="container">
    <div class="hero section" :style="{ backgroundImage: `url(${heroBgWhite})` }">
      <div class="hero-content">
        <h1 class="title">TinyVue 智能组件库</h1>
        <p class="subtitle">轻量 高性能 智能化</p>
        <p class="description">一款支持主流前端框架、AI对话框、MCP Host 和智能体搭建平台</p>
        <div class="cta-group">
          <a href="https://opentiny.design/tiny-vue" target="_blank" class="btn primary">立即体验</a>
          <a href="https://docs.opentiny.design/tiny-vue/guide/introduce.html" target="_blank" class="btn secondary"
            >产品文档</a
          >
        </div>
      </div>
      <div class="hero-img">
        <img :src="tinyvueThemeCustomization" alt="TinyVue 智能组件库" />
      </div>
    </div>

    <!-- Feature 1: 跨端跨框架 -->
    <section class="feature-section section bg-tech-1">
      <div class="feature-header pad-t40">
        <h2 class="title feature-title">跨端跨框架</h2>
        <p class="description">
          相同的组件代码可在不同的Vue版本和设备平台上运行，显著降低了企业应用的维护成本，无需重复开发，大幅提升开发效率
        </p>
      </div>
      <div class="feature-content pad-t40">
        <div v-for="(item, index) in crossPlatformFeatures" :key="index" class="feature-text">
          <div class="title-logo">
            <img data-v-d881ea2c="" class="app-logo" :src="item.icon" />
            <h2 class="title feature-sub-title">{{ item.title }}</h2>
          </div>
          <p class="description">
            {{ item.description }}
          </p>
        </div>
      </div>
      <div class="feature-visual pad-t40">
        <img :src="featureCrossPlatform" alt="跨端跨框架" class="floating-img" />
      </div>
    </section>

    <!-- Feature 2: 丰富的组件 -->
    <section class="feature-section section bg-tech-2" :style="{ backgroundImage: `url(${featureBgWhite2})` }">
      <div class="feature-header">
        <h2 class="title feature-title">丰富的组件</h2>
        <p class="description">持续更新迭代，满足企业级应用需求。</p>
      </div>
      <div class="flex-row" style="width: 550px; gap: 120px">
        <div class="flex-row-column">
          <div class="title feature-text-title title-number">130+</div>
          <div class="title-logo">
            <img :src="iconWindow" width="18" height="18" alt="桌面端组件" />
            <div class="title-logo-text">桌面端组件</div>
          </div>
        </div>
        <div class="flex-row-column">
          <div class="title feature-text-title title-number">30+</div>
          <div class="title-logo">
            <img :src="iconPhone" width="18" height="18" alt="移动端组件" />
            <div class="title-logo-text">移动端组件</div>
          </div>
        </div>
      </div>
      <div class="feature-content component-demo-container pad-t40">
        <div class="component-demo component-demo-search">
          <div class="component-demo-title title">DatePicker</div>
          <tiny-input placeholder="YYYY/MM/DD" />
          <tiny-date-panel class="custom-date-panel" />
        </div>
        <div class="component-demo">
          <div class="component-demo-title title">TreeMenu</div>
          <tiny-tree-menu
            class="demo-tree-menu"
            node-key="id"
            :data="treeData"
            :default-expanded-keys="expandeArr"
            :default-expanded-keys-highlight="highlight"
          >
            <template #default="slotScope">
              <a>
                {{ slotScope.data.label }}
              </a>
              <img
                v-if="slotScope.data.label === 'TinyVue'"
                width="11"
                height="11"
                :src="iconShare"
                alt="share"
                class="tree-menu-icon"
              />
            </template>
          </tiny-tree-menu>
        </div>
        <div class="component-demo-search">
          <div class="component-demo">
            <div class="component-demo-title title">Search</div>
            <tiny-search placeholder="Enter a keywords" clearable>
              <template #prefix>
                <tiny-icon-search />
              </template>
            </tiny-search>
          </div>
          <div class="component-demo">
            <div class="component-demo-title title">Button</div>
            <tiny-row>
              <tiny-button type="primary" :icon="TinyIconEditor" circle></tiny-button>
              <tiny-button type="success" :icon="TinyIconYes" circle></tiny-button>
              <tiny-button type="info" :icon="TinyIconMail" circle></tiny-button>
              <tiny-button type="warning" :icon="TinyIconStarO" circle></tiny-button>
              <tiny-button type="danger" :icon="TinyIconDeleteL" circle></tiny-button>
              <tiny-button> Button </tiny-button>
            </tiny-row>
          </div>
          <div class="component-demo">
            <div class="component-demo-title title">Slider</div>
            <tiny-slider
              v-model="slidValue"
              :show-input="true"
              :min="0"
              :max="500"
              :step="100"
              unit="Mbit/s"
            ></tiny-slider>
          </div>
        </div>
        <div class="component-demo-search">
          <div class="component-demo">
            <div class="component-demo-title title">Steps</div>
            <tiny-steps line :data="stepsData" :active="active"></tiny-steps>
          </div>
          <div class="component-demo">
            <div class="component-demo-title title">Alert</div>
            <tiny-row>
              <tiny-alert :closable="false">
                <template #description
                  >文本占位，关键字
                  <tiny-link>
                    <template #icon>
                      <tiny-icon-fillet-external-link class="tiny-svg-link"></tiny-icon-fillet-external-link>
                    </template>
                    文本链接
                  </tiny-link>
                </template>
              </tiny-alert>
              <tiny-alert type="warning" :closable="false">
                <template #description>安全组触发规则告警<tiny-link type="warning">提示警告文字</tiny-link> </template>
              </tiny-alert>
              <tiny-alert type="error" :closable="false">
                <template #description
                  >安全组触发规则告警<tiny-link type="danger">错误、失败类文字</tiny-link>
                </template>
              </tiny-alert>
              <tiny-alert type="success" :closable="false">
                <template #description
                  >文本占位，关键字
                  <tiny-link>
                    <template #icon>
                      <tiny-icon-fillet-external-link class="tiny-svg-link"></tiny-icon-fillet-external-link>
                    </template>
                    文本链接
                  </tiny-link>
                </template>
              </tiny-alert>
            </tiny-row>
          </div>
        </div>

        <div class="component-demo-search component-demo-anchor">
          <div class="component-demo">
            <div class="component-demo-title title">Anchor</div>
            <tiny-anchor :links="anchorData" type="dot"></tiny-anchor>
          </div>
          <div class="component-demo">
            <div class="component-demo-title title">Checkbox</div>
            <tiny-row>
              <tiny-checkbox :indeterminate="true">全选</tiny-checkbox>
              <tiny-checkbox>选项2</tiny-checkbox>
            </tiny-row>
          </div>
        </div>
      </div>
    </section>

    <!-- Feature 3: 配置式组件 -->
    <section class="feature-section section bg-tech-1" :style="{ backgroundImage: `url(${featureBgWhite1})` }">
      <div class="feature-header pad-t40">
        <h2 class="title feature-title">配置式组件</h2>
        <p class="description">支持模板式和配置式两种使用方式， 配置式组件让复杂场景的开发更简单高效。</p>
      </div>
      <div class="feature-content pad-t40">
        <div class="flex-row gap-30">
          <div
            v-for="(item, index) in configArchitecture"
            :key="index"
            class="flex-row flex-row-column gap-30 config-item"
          >
            <div
              :style="{
                width: '100%',
                height: '280px',
                borderRadius: '10px 10px 0 0',
                backgroundColor: item.backgroundColor
              }"
            ></div>
            <div class="title feature-sub-title pad-0-30">{{ item.title }}</div>
            <div class="description feature-text-description pad-0-30">
              {{ item.description }}
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Feature 4: 强大的主题定制 -->
    <section class="feature-section section bg-tech-2 reverse" :style="{ backgroundImage: `url(${featureBgWhite2})` }">
      <div class="feature-header pad-t40">
        <h2 class="title feature-title">强大的主题定制</h2>
        <p class="description">支持灵活的主题配置和定制能力。轻松适配企业品牌风格，打造独特视觉体验。</p>
      </div>
      <div class="feature-content">
        <div class="feature-visual pad-t40">
          <img :src="tinyvueThemeCustomization" alt="主题定制" class="floating-img delay-3" />
        </div>
        <div class="feature-visual pad-t40">
          <img :src="tinyvueThemeCustomization" alt="主题定制" class="floating-img delay-3" />
        </div>
      </div>
    </section>

    <!-- MCP Section -->
    <section class="mcp-section section">
      <div class="feature-header pad-t40">
        <h2 class="title feature-title">智能化生成式 UI</h2>
        <p class="description">全新升级的 TinyVue，引入 AI 智能化能力，让组件开发更智能、更高效</p>
      </div>
      <div class="mcp-content">
        <div class="feature-grid">
          <div v-for="(item, index) in mcpFeatures" :key="index" class="feature-card">
            <div class="feature-icon">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
                <path :d="item.iconPath" />
              </svg>
            </div>
            <h3>{{ item.title }}</h3>
            <p>{{ item.description }}</p>
          </div>
        </div>
      </div>
      <div class="mcp-image pad-t40"></div>
    </section>
  </div>
</template>

<style scoped lang="less">
.container {
  width: 100%;
  overflow-x: hidden;
  background-color: var(--bg-color);
}

.pad-0-30 {
  padding-left: 30px;
  padding-right: 30px;
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

.mcp-image {
  width: 100%;
  height: 100%;
  max-width: 1400px;
  display: flex;
  justify-content: center;
  margin: 0 40px;
  align-items: center;
  background: #eee;
  height: 400px;
}

/* Backgrounds */
.hero {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 40px;
  padding: 40px 80px 0 80px;
}

.gap-30 {
  gap: 30px;
}

.bg-tech-1 {
  background-size: cover;
  background-position: center;
}

.bg-tech-2 {
  background-size: cover;
  background-position: center;
}

.mcp-section {
  background-size: cover;
  background-position: center;
  justify-content: space-around;
}

/* Hide underline for MCP title */
.mcp-section .section-title.light::after {
  display: none;
}

.mcp-content {
  max-width: 1400px;
  width: 100%;
  padding: 0 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

/* Hero Content */
.hero-content {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  padding: 20px;
  padding-top: 0;
  animation: fadeInUp 1s ease-out;
}

.title {
  font-size: 50px;
  font-weight: 700;
  color: #191919;
  letter-spacing: 5px;
  line-height: 1.2;
  padding-bottom: 20px;
}

.title-number {
  font-size: 44px;
  font-weight: 600;
  color: #1476ff;
  letter-spacing: 2px;
  line-height: 1.2;
  padding-bottom: 20px;
}

.title-logo-text {
  font-size: 16px;
  color: #808080;
  letter-spacing: 2px;
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
  line-height: 1.5;
  color: var(--text-secondary);
  font-weight: 400;
}

.cta-group {
  display: flex;
  gap: 30px;
}

.btn {
  padding: 10px 30px;
  border-radius: 30px;
  font-size: 14px;
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
  backdrop-filter: blur(10px);
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

.component-demo-search {
  display: flex;
  flex-direction: column;
  gap: 20px;
  min-width: 346px;
  flex: 1;
}

.component-demo-anchor {
  display: flex;
  flex-direction: column;
  gap: 20px;
  min-width: 186px;
}

.component-demo {
  display: flex;
  flex-direction: column;
  padding: 30px 25px;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  flex: 1;

  .tiny-tree-menu:before {
    border-right: none;
  }

  :deep(.tiny-alert__description) {
    display: flex;
    gap: 3px;
    min-width: 242px;
  }

  :deep(.tiny-steps-line) {
    .tiny-steps-block .title {
      display: none;
    }
    .tiny-steps-block.not-vertical {
      min-width: 70px;
    }
  }
}
.component-demo-title {
  font-size: 20px;
  font-weight: 600;
  color: #191919;
  letter-spacing: 2px;
  line-height: 1.2;
  padding-bottom: 20px;
}

.tree-menu-icon {
  margin-left: 5px;
}

.custom-date-panel {
  display: block !important;
  width: 100%;
  box-shadow: none;
}

.flex-row {
  display: flex;
  align-items: center;
  justify-content: center;
}

.flex-row-column {
  flex-direction: column;
}

.config-item {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  align-items: baseline;
  gap: 20px;

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2);
  }
  .description {
    color: #808080;
  }

  .title {
    font-size: 24px;
  }
}

/* Feature Sections */
.feature-content {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 1400px;
  width: 100%;
  height: 100%;
}

.component-demo-container {
  display: flex;
  align-items: stretch;
  gap: 20px;
  justify-content: center;
}

.pad-t40 {
  padding-top: 40px;
}

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

.reverse .feature-content {
  flex-direction: row-reverse;
}

.feature-text {
  flex: 1;
  color: var(--text-primary);
  padding: 0 30px;

  .description {
    font-size: 14px;
    letter-spacing: 2px;
    color: #808080;
  }
}

.section-title.light {
  font-size: 51px;
  font-weight: 700;
  margin-bottom: 40px;
  color: var(--text-primary);
  text-shadow: none;
  position: relative;
  white-space: nowrap;
}

.section-title.light::after {
  content: '';
  display: block;
  width: 80px;
  height: 6px;
  background: var(--primary-gradient);
  margin-top: 20px;
  border-radius: 3px;
}

.feature-desc {
  font-size: 26px;
  line-height: 1.8;
  color: var(--text-secondary);
  font-weight: 300;
}

.feature-visual {
  display: flex;
  justify-content: center;
  align-items: center;
}

.floating-img {
  width: 90%;
  max-width: 600px;
  filter: drop-shadow(0 20px 40px rgba(0, 0, 0, 0.15));
  animation: float 6s ease-in-out infinite;
  transition: transform 0.5s ease;
  border-radius: 24px; /* Add rounded corners */
}

.floating-img:hover {
  transform: scale(1.05);
}

.delay-1 {
  animation-delay: 1s;
}
.delay-2 {
  animation-delay: 2s;
}
.delay-3 {
  animation-delay: 3s;
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

/* MCP Section Styles */
.mcp-desc {
  font-size: 22px;
  color: var(--text-secondary);
  margin-bottom: 60px;
  max-width: 800px;
}

.feature-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 30px;
  width: 100%;
  max-width: 1400px;
}

.feature-card {
  background: #f0f0f0;
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 30px;
  text-align: left;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.8);
}

.feature-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(94, 124, 226, 0.15);
  border-color: rgba(94, 124, 226, 0.3);
}

.feature-icon {
  width: 60px;
  height: 60px;
  margin-bottom: 24px;
  color: var(--primary-color);
}

.feature-icon svg {
  width: 100%;
  height: 100%;
}

.feature-card h3 {
  font-size: 21px;
  font-weight: 600;
  margin-bottom: 16px;
  color: var(--text-primary);
}

.feature-card p {
  font-size: 16px;
  line-height: 1.6;
  color: var(--text-secondary);
}

.footer-mini {
  position: relative;
  z-index: 1;
  width: 100%;
  text-align: center;
  padding: 20px;
  background: rgba(255, 255, 255, 0.9);
  color: var(--text-secondary);
  font-size: 14px;
  backdrop-filter: blur(10px);
  border-top: 1px solid rgba(0, 0, 0, 0.05);
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

@media (max-width: 1024px) {
  .feature-content {
    flex-direction: column;
    text-align: center;
    padding: 0 30px;
    justify-content: center;
  }
  .reverse .feature-content {
    flex-direction: column;
  }
  .feature-text {
    padding: 0;
    margin-bottom: 50px;
  }
  .feature-visual {
    width: 100%;
  }
  .section-title.light {
    font-size: 48px;
  }
  .section-title.light::after {
    margin: 20px auto 0;
  }
  .title {
    font-size: 56px;
  }
  .feature-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .feature-grid {
    grid-template-columns: 1fr;
  }
}
@media (max-width: 768px) {
  .title {
    font-size: 36px;
    margin-top: -60px; /* Adjust for smaller screen */
  }

  .subtitle {
    font-size: 24px;
    margin-bottom: 32px;
  }

  .hero-content {
    padding: 20px;
  }

  .feature-content {
    flex-direction: column;
    padding: 0 20px;
    text-align: center;
    justify-content: center;
  }

  .reverse .feature-content {
    flex-direction: column;
  }

  .feature-text {
    padding: 0;
    margin-bottom: 40px;
  }

  .section-title.light {
    font-size: 32px;
  }

  .section-title.light::after {
    margin: 20px auto 0;
  }

  .feature-desc {
    font-size: 19px;
  }

  .feature-visual {
    width: 100%;
  }

  .mcp-section {
    height: auto;
    min-height: 100vh;
    padding: 60px 0;
    overflow-y: visible;
  }

  .mcp-content {
    padding: 0 20px;
    height: auto;
  }

  .feature-grid {
    grid-template-columns: 1fr; /* Stack cards on mobile */
    gap: 20px;
    margin-bottom: 40px;
  }

  .feature-card {
    padding: 30px 20px;
  }

  .cta-group {
    flex-direction: column;
    gap: 15px;
  }

  .btn {
    width: 100%;
    padding: 12px 0;
  }
}
</style>
