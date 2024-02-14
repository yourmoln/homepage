<template>
  <n-layout />
  <n-layout
    has-sider
    id="home"
    :style="'background:url(' + bgimg + ') center center no-repeat;'"
  >
    <n-modal v-model:show="bg">
      <n-card
        style="width: 600px"
        title="更换壁纸"
        :bordered="false"
        size="huge"
        role="dialog"
        aria-modal="true"
        closable
        @close="closebgimgset"
      >
        <n-input v-model:value="bgimg" type="text" placeholder="壁纸链接" />
        <template #footer>
          <n-button @click="resetBgimg">重置链接</n-button>
        </template>
      </n-card>
    </n-modal>
    <n-modal v-model:show="login">
      <n-card class="login">
        <n-tabs
          class="card-tabs"
          default-value="signin"
          size="large"
          animated
          pane-wrapper-style="margin: 0 -4px"
          pane-style="padding-left: 4px; padding-right: 4px; box-sizing: border-box;"
        >
          <n-tab-pane name="signin" tab="登录">
            <n-form>
              <n-form-item-row label="用户名">
                <n-input />
              </n-form-item-row>
              <n-form-item-row label="密码">
                <n-input />
              </n-form-item-row>
            </n-form>
            <n-button type="primary" block secondary strong> 登录 </n-button>
          </n-tab-pane>
          <n-tab-pane name="signup" tab="注册">
            <n-form>
              <n-form-item-row label="用户名">
                <n-input />
              </n-form-item-row>
              <n-form-item-row label="密码">
                <n-input />
              </n-form-item-row>
              <n-form-item-row label="重复密码">
                <n-input />
              </n-form-item-row>
            </n-form>
            <n-button type="primary" block secondary strong> 注册 </n-button>
          </n-tab-pane>
        </n-tabs>
      </n-card>
    </n-modal>
    <n-layout-sider
      class="menu"
      bordered
      collapse-mode="width"
      :collapsed-width="64"
      :width="240"
      :collapsed="collapsed"
      show-trigger
      @collapse="collapsed = true"
      @expand="collapsed = false"
    >
      <n-menu
        :collapsed="collapsed"
        :collapsed-width="64"
        :collapsed-icon-size="22"
        :options="menuOptions"
        key-field="whateverKey"
        label-field="whateverLabel"
        children-field="whateverChildren"
      />
    </n-layout-sider>

    <n-space vertical class="body">
      <n-flex justify="space-between" class="top">
        <p></p>
        <n-gradient-text :size="34" type="info">
          <b style="text-stroke: 1px rgb(255, 255, 255)">
            <n-time format="HH:mm:ss" time-zone="Asia/Shanghai" />
          </b>
        </n-gradient-text>
        <n-avatar @click="login = true" lazy round :size="48" src="heita.svg" />
      </n-flex>
      <div class="main">
        <div class="search">
          <n-input
            round
            size="large"
            v-model:value="search_text"
            :placeholder="'在' + search_engine_name + '中搜索'"
            @keyup.enter.native="search()"
          >
            <template #suffix>
              <n-dialog-provider>
                <n-icon @click="search" :component="SearchOutline" />
              </n-dialog-provider>
            </template>
          </n-input>
        </div>
        <n-carousel autoplay>
          <n-card class="autocard" title="卡片1"> 卡片内容1 </n-card>
          <n-card class="autocard" title="卡片2"> 卡片内容2 </n-card>
          <n-card class="autocard" title="卡片3"> 卡片内容3 </n-card>
        </n-carousel>
        <n-card title="卡片"> 卡片内容 </n-card>
        <n-card title="卡片"> 卡片内容 </n-card>
        <n-card title="卡片"> 卡片内容 </n-card>
      </div>
    </n-space>
  </n-layout>
</template>

<script lang="ts">
import { SearchOutline } from "@vicons/ionicons5";
import { defineComponent, h, ref, Component } from "vue";
import { NCard, NIcon, NInput } from "naive-ui";
import type { MenuOption } from "naive-ui";
import {
  Search as SearchIcon,
  LogoGithub as GithubIcon,
  MusicalNotes as MusicIcon,
  ImageOutline as ImageIcon,
  CloudyOutline as CloudyIcon,
} from "@vicons/ionicons5";

function renderIcon(icon: Component) {
  return () => h(NIcon, null, { default: () => h(icon) });
}
const search_engine_name = ref("必应");
const bing = "https://cn.bing.com/search?q=";
const baidu = "https://www.baidu.com/s?wd=";
const sogou = "https://www.sogou.com/web?query=";
const github = "https://github.com/search?q=";
const search_text = ref("");
const search_engine = ref(bing);
if (localStorage.getItem("bgimg") == null) {
  localStorage.setItem("bgimg", "https://t.mwm.moe/pc/");
}
const resetBgimg = () => {
  localStorage.setItem("bgimg", "https://t.mwm.moe/pc/");
  bgimg.value = localStorage.getItem("bgimg");
};
const bgimg = ref(localStorage.getItem("bgimg"));
const bg = ref(false);
const login = ref(false);
const search = () => {
  if (search_text.value == "") {
    // alert("搜索内容不能为空")
  } else {
    window.open(search_engine.value + search_text.value);
  }
};
const closebgimgset = () => {
  bg.value = false;
  localStorage.setItem("bgimg", bgimg.value!);
};
const menuOptions: MenuOption[] = [
  {
    whateverLabel: "搜索设置",
    whateverKey: "search-setting",
    icon: renderIcon(SearchIcon),
    whateverChildren: [
      {
        type: "group",
        whateverLabel: "搜索设置",
        whateverKey: "search-setting",
        whateverChildren: [
          {
            whateverLabel: () =>
              h(
                "a",
                {
                  onClick: () => {
                    console.log("bing");
                    search_engine.value = bing;
                    search_engine_name.value = "必应";
                  },
                },
                "必应"
              ),
            whateverKey: "bing",
            icon: renderIcon(SearchIcon),
          },
          {
            whateverLabel: () =>
              h(
                "a",
                {
                  onClick: () => {
                    console.log("baidu");
                    search_engine.value = baidu;
                    search_engine_name.value = "百度";
                  },
                },
                "百度"
              ),
            whateverKey: "baidu",
            icon: renderIcon(SearchIcon),
          },
          {
            whateverLabel: () =>
              h(
                "a",
                {
                  onClick: () => {
                    console.log("sogou");
                    search_engine.value = sogou;
                    search_engine_name.value = "搜狗";
                  },
                },
                "搜狗"
              ),
            whateverKey: "sogou",
            icon: renderIcon(SearchIcon),
          },
          {
            whateverLabel: () =>
              h(
                "a",
                {
                  onClick: () => {
                    console.log("github");
                    search_engine.value = github;
                    search_engine_name.value = "github";
                  },
                },
                "github"
              ),
            whateverKey: "github",
            icon: renderIcon(GithubIcon),
          },
        ],
      },
    ],
  },
  {
    whateverLabel: "音乐",
    whateverKey: "Music",
    icon: renderIcon(MusicIcon),
    disabled: false,
    whateverChildren: [
      {
        type: "group",
        whateverLabel: "音乐播放",
        whateverKey: "search-setting",
        whateverChildren: [
          {
            whateverLabel: () =>
              h(
                "a",
                {
                  onClick: () => {
                    window.open("https://y.qq.com/");
                  },
                },
                "QQ音乐"
              ),
            whateverKey: "bing",
            //icon: renderIcon(SearchIcon)
          },
        ],
      },
    ],
  },
  {
    whateverLabel: "壁纸设置",
    whateverKey: "bgimg-setting",
    icon: renderIcon(ImageIcon),
    disabled: false,
    whateverChildren: [
      {
        type: "group",
        whateverLabel: "壁纸设置",
        whateverKey: "bgimg-setting",
        whateverChildren: [
          {
            whateverLabel: () =>
              h(
                "a",
                {
                  onClick: () => {
                    bg.value = true;
                  },
                },
                "更换壁纸"
              ),
            whateverKey: "bgimg-setting",
          },
        ],
      },
    ],
  },
  {
    whateverLabel: "天气",
    whateverKey: "weather",
    icon: renderIcon(CloudyIcon),
    disabled: false,
    whateverChildren: [
      {
        type: "group",
        whateverLabel: "天气",
        whateverKey: "weather",
        whateverChildren: [
          {
            whateverLabel: () =>
              h(
                "iframe",
                {
                  src: "https://weather.cma.cn/",
                },
                ""
              ),
            whateverKey: "weather",
          },
        ],
      },
    ],
  },
];

export default defineComponent({
  setup() {
    return {
      collapsed: ref(true),
      menuOptions,
      SearchOutline,
      search_text,
      search,
      search_engine_name,
      bgimg,
      bg,
      login,
      closebgimgset,
      resetBgimg,
    };
  },
});
</script>

<style>
.body {
  margin: 0 auto;
  width: 100vw;
}
.main {
  opacity: 0.9;
  width: 80vw;
  margin: 0 auto;
}
.autocard {
  background-color: rgb(223, 185, 241);
}
.search {
  opacity: 0.7;

  margin: 3% auto;
  width: 60vw;
}
.menu {
  opacity: 0.7;
}
.top {
  padding-top: 1%;
  padding-right: 1%;
  padding-left: 1%;
}
.login {
  width: 30vw;
  min-width: 300px;
}
#home {
  width: 100%;
  height: 100vh;
  /* background: url("https://t.mwm.moe/pc/") center center no-repeat; */
  background-size: 100% 100%;
  position: fixed;
}
</style>
