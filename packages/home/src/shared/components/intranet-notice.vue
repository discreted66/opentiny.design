<template>
  <div class="intranet-notice-mask" @click.self="handleClose">
    <div class="intranet-notice-panel">
      <button class="intranet-notice-close" type="button" @click="handleClose">✕</button>
      <div class="intranet-notice-body">
        <div class="intranet-notice-top">
          <h1 class="intranet-notice-title">{{ title }}</h1>
          <h2 class="intranet-notice-subtitle">{{ subtitle }}</h2>
          <p class="intranet-notice-desc">
            {{ description }}
          </p>
        </div>
        <div class="intranet-notice-bottom">
          <div class="intranet-notice-col">
            <template v-for="(item, index) in items" :key="item.name">
              <div class="intranet-notice-item">
                <div class="intranet-notice-item-head">
                  <img
                    :src="$pub(`images/${item.logo}.svg`)"
                    :alt="item.name"
                    class="intranet-notice-icon"
                  />
                  <span class="intranet-notice-name">{{ item.name }}</span>
                </div>
                <p class="intranet-notice-item-desc">
                  {{ item.description }}
                  <span class="intranet-notice-contact">{{ item.contact }}</span>
                </p>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { $pub } from "@/tools";

const emit = defineEmits(["close"]);

const title = "全新推出";
const subtitle = "企业前端开发解决方案";
const description =
  "OpenTiny NEXT 是一个企业智能前端开发解决方案，以生成式 UI + WebMCP 两大自主核心技术为基础，加速企业应用的智能化改造。";

const items = [
  {
    logo: "tiny-vue",
    name: "TinyVue",
    description: "提供 Agent Skills 技能包，让 AI 使用 TinyVue 组件快速搭建 Web 应用。",
    contact: "接口人：曾令卡 00507287",
  },
  {
    logo: "tiny-engine",
    name: "TinyEngine",
    description: "AI 时代的智能低代码基座。支持在线实时构建、支持设计器命令行二次开发。",
    contact: "接口人：伍其和 00464684",
  },
  {
    logo: "tiny-robot",
    name: "TinyRobot",
    description: "专为 AI 应用构建的前端交互框架，提供丰富 AI 交互组件。",
    contact: "接口人：伍其和 00464684",
  },
  {
    logo: "next-sdk",
    name: "NEXT-SDKs",
    description:
      "一套面向前端智能应用的开发工具包，基于 MCP 协议实现 AI 代替人操作 Web 应用。",
    contact: "接口人：曾令卡 00507287",
  },
  {
    logo: "GenUI-SDK",
    name: "GenUI-SDK",
    description: "告别“纯文本”，生成式 UI 开启 AI 交互的下一代革命。",
    contact: "接口人：林瑞虹 00941564",
  },
];

const handleClose = () => {
  emit("close");
};
</script>

<style scoped lang="less">
.intranet-notice-mask {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.intranet-notice-panel {
  position: relative;
  max-width: 1000px;
  width: 90%;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 18px 50px rgba(0, 0, 0, 0.16);
}

.intranet-notice-close {
  position: absolute;
  top: 12px;
  right: 12px;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: none;
  background: rgba(0, 0, 0, 0.04);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  color: #191919;
}

.intranet-notice-body {
  padding: 58px 68px;
}

.intranet-notice-top {
  padding-bottom: 57px;
  border-bottom: 1px solid rgba(151, 164, 224, 0.3);
}

.intranet-notice-title {
  font-size: 56px;
  color: #808080;
  line-height: 75px;
  font-weight: 200;
}

.intranet-notice-subtitle {
  font-size: 42px;
  font-weight: 700;
  line-height: 56px;
  margin: 0 0 12px 0;
  background: linear-gradient(90deg, #cb43a8, #2c5fef 56%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.intranet-notice-desc {
  font-size: 18px;
  color: #595959;
  line-height: 31px;
  margin: 0;
}

.intranet-notice-bottom {
  display: flex;
}

.intranet-notice-col {
  padding-top: 58px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: auto auto;
  gap: 32px;
}

.intranet-notice-item {
  padding: 16px;
  background: #f7f8fa;
  border-radius: 8px;
}

.intranet-notice-item-head {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
}

.intranet-notice-icon {
  width: 22px;
  height: 22px;
  object-fit: contain;
}

.intranet-notice-name {
  font-size: 18px;
  font-weight: 700;
  color: #595959;
  line-height: 22px;
}

.intranet-notice-item-desc {
  font-size: 16px;
  font-weight: 400;
  color: #808080;
  line-height: 24px;
}

.intranet-notice-contact {
  font-size: 12px;
  color: #8a8a8a;
  margin: 0;
}

@media screen and (max-width: 600px) {
  .intranet-notice-body {
    padding: 20px 16px 16px;
  }

  .intranet-notice-bottom {
    grid-template-columns: 1fr;
  }
}
</style>
