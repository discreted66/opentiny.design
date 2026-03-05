<script lang="jsx" setup>
import { onMounted, ref } from "vue";
import { ConfigProvider } from "@opentiny/vue";
import designSmbConfig from "@opentiny/vue-design-smb";
import hljs from "highlight.js/lib/core";
// 示例中代码高亮
import javascript from "highlight.js/lib/languages/javascript";
import css from "highlight.js/lib/languages/css";
import html from "highlight.js/lib/languages/xml";

import { findParent, getRoutePath } from "./tools";
import { router } from "./router";
import IntranetNotice from "@/shared/components/intranet-notice.vue";

// import '@/genui-sdk/index.css'

hljs.registerLanguage("javascript", javascript);
hljs.registerLanguage("css", css);
hljs.registerLanguage("html", html);

// 内网判断：非公网域名（opentiny.design）时视为内网
const isIntranet = () => {
  if (typeof window === "undefined") return false;
  const host = window.location.hostname;
  return (
    host === "localhost" ||
    host === "127.0.0.1" ||
    (host !== "opentiny.design" && !host.endsWith(".opentiny.design"))
  );
};

const showIntranetModal = ref(false);

const jumpByRouter = (event) => {
  const isRouterDom = (parent) => {
    const { tagName, href, target } = parent;
    if (
      tagName?.toLowerCase() === "a" &&
      href?.startsWith?.(location.origin) &&
      target !== "_blank"
    ) {
      const routerPath = getRoutePath(href);
      if (routerPath) {
        event.preventDefault();
        router.push(routerPath);
        return true;
      }
    }
    return false;
  };
  if (!isRouterDom(event.target)) {
    findParent(event.target, isRouterDom);
  }
};

onMounted(() => {
  document.querySelector("#header")?.addEventListener("click", jumpByRouter, true);
  if (isIntranet()) {
    showIntranetModal.value = true;
  }
});
</script>

<template>
  <div class="hp100">
    <config-provider :design="designSmbConfig" class="hp100">
      <router-view />
      <IntranetNotice v-if="showIntranetModal" @close="showIntranetModal = false" />
    </config-provider>
  </div>
</template>
