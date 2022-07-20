<script lang="ts" setup>
import {computed, h, ref} from "vue";
import {
  createDiscreteApi,
  darkTheme,
  lightTheme,
  MenuOption,
  NAlert,
  NButton,
  NCard,
  NConfigProvider,
  NDivider,
  NGrid,
  NGridItem,
  NImage,
  NLayout,
  NLayoutContent,
  NLayoutFooter,
  NLayoutHeader,
  NMenu,
  NSpace,
  NText,
  useOsTheme
} from "naive-ui";
import Logo from "../public/logo.png";
import Banner from "../public/banner.png";

const theme = computed(() => {
  return useOsTheme().value === 'dark' ? darkTheme : lightTheme;
});

const menuOptions = [
  {
    label: '几何冲刺玩家网',
    key: 'home',
    icon: () => h(NImage, {
      src: Logo,
      width: 20
    }),
    url: '/'
  }
] as MenuOption[];

function handleMenuClick(label: string, item: MenuOption) {
  location.href = item.url as string;
}

function checkMobile() {
  isMobile.value = screen.width <= 768;
}

const isMobile = ref();
window.addEventListener('resize', () => {
  checkMobile();
});

checkMobile();

function showJoinGroupDialog(id: number, key: string) {
  const instance = createDiscreteApi(['dialog'], {
    configProviderProps: {
      theme: theme.value
    }
  });

  function destroy() {
    setTimeout(() => {
      instance.unmount()
    }, 1000);
  }

  instance.dialog.create({
    showIcon: false,
    title: '群号: ' + id,
    content: () => h(NImage, {
      src: '/public/' + id + '.png',
      imgProps: {
        class: 'w-full'
      }
    }),
    onClose: destroy,
    onMaskClick: destroy,
    closeOnEsc: false,
    action: () => h(NButton, {
      tag: 'a',
      href: 'https://qm.qq.com/cgi-bin/qm/qr?k=' + key
    }, () => '点我进群')
  });
}
</script>

<template>
  <n-config-provider :theme="theme" class="h-full">
    <n-layout class="h-full">
      <n-layout-header>
        <n-menu :options="menuOptions" mode="horizontal" @update:value="handleMenuClick"/>
      </n-layout-header>

      <n-layout-content class="p-[24px] pb-20">
        <div class="mx-auto w-full mb-2.5 text-center">
          <n-image :img-props="{ class: 'w-full' }" class="lg:w-1/2" :src="Banner"/>
        </div>

        <n-alert class="lg:w-1/3 mb-5 mx-auto" closable type="info">
          如你所见, 欢迎来到崭新的
          <n-button href="/" tag="a" text type="primary">几何冲刺玩家网</n-button>
          !
        </n-alert>

        <div class="lg:w-1/2 mx-auto">
          <n-divider>相关链接</n-divider>

          <n-space vertical>
            <n-card>
              <n-space :vertical="isMobile" justify="center">
                <n-button href="https://robtopgames.com" tag="a">Robtop Games</n-button>
                <n-button href="https://pointercrate.com" tag="a">PointerCrate</n-button>
                <n-button href="https://newgrounds.com" tag="a">NewGrounds</n-button>

                <n-button href="https://www.boomlings.com/database/accounts/accountManagement.php" tag="a">
                  官服账号管理
                </n-button>

                <n-divider v-if="!isMobile" vertical/>

                <n-button href="https://nsc.geometrydashchinese.com" tag="a">
                  NewGrounds Song Collection
                </n-button>
              </n-space>
            </n-card>

            <n-grid :x-gap="10" :y-gap="10" cols="1 768:2">
              <n-grid-item>
                <n-card title="Geometry Dash Chinese (GDCN)">
                  <template #header-extra>
                    <n-button href="https://qm.qq.com/cgi-bin/qm/qr?k=l5AT2gbxhx1uXDV8u1HC_XT9LCIGseLz" tag="a"
                              text type="primary">
                      交流群
                    </n-button>
                  </template>

                  <n-text type="info">GDCN 是对 GDProxy, NGProxy, GDCS 的统称</n-text>
                  <br/><br/>
                </n-card>
              </n-grid-item>
              <n-grid-item>
                <n-card title="Geometry Dash Chinese Server (GDCS)">
                  <template #header-extra>
                    <n-button href="https://gf.geometrydashchinese.com" tag="a" text type="primary">查看</n-button>
                  </template>

                  <n-text type="info">
                    GDCS 是一个由
                    <n-button href="https://github.com/WOSHIZHAZHA120" tag="a" text type="primary">渣渣120</n-button>
                    独立开发的开源私服系统

                    <br>

                    仓库地址:
                    <n-button href="https://github.com/Geometry-Dash-Chinese/Geometry-Dash-Chinese" tag="a" text
                              type="primary">Geometry-Dash-Chinese/Geometry-Dash-Chinese
                    </n-button>
                  </n-text>
                </n-card>
              </n-grid-item>
              <n-grid-item>
                <n-card title="Geometry Dash Proxy (GDProxy)">
                  <template #header-extra>
                    <n-button href="https://dl.geometrydashchinese.com" tag="a" text type="primary">查看</n-button>
                  </template>

                  <n-text type="info">
                    GDProxy 是一个官服加速器, 可以加速
                    <n-text depth="3">(或减速?)</n-text>
                    您链接官服的速度
                  </n-text>
                </n-card>
              </n-grid-item>
              <n-grid-item>
                <n-card title="Newgrounds Proxy (NGProxy)">
                  <template #header-extra>
                    <n-button href="https://ng.geometrydashchinese.com" tag="a" text type="primary">查看</n-button>
                  </template>

                  <n-text type="info">
                    NGProxy 是一个下歌加速服务, 可以加速
                    <n-text depth="3">(或减速?)</n-text>
                    您下载歌曲的速度
                  </n-text>
                </n-card>
              </n-grid-item>
            </n-grid>
          </n-space>

          <n-divider>社区</n-divider>

          <n-card>
            <n-space :vertical="isMobile" justify="space-evenly">
              <n-space :vertical="isMobile" class="items-center">
                <n-text depth="3">百度贴吧</n-text>
                <n-button href="https://ng.geometrydashchinese.com" tag="a">几何冲刺吧</n-button>
              </n-space>

              <n-divider v-if="isMobile"/>

              <n-space :vertical="isMobile" class="items-center">
                <n-text depth="3">QQ群</n-text>
                <n-button @click="showJoinGroupDialog(569801410, 'gmeT9wov61xLRlGiQZZzXc-IxioxBbHG')">几何冲刺萌新聚集地
                </n-button>
                <n-button @click="showJoinGroupDialog(302185016, 'WY64FZrDI0wSslR7pegIJuMhpe-qKO7w')">几何冲刺爱好者联盟
                </n-button>
              </n-space>
            </n-space>
          </n-card>

          <n-divider>开源项目</n-divider>

          <n-card>
            <n-space justify="center">
              <n-button href="https://github.com/Geometry-Dash-Chinese/Geometry-Dash-Chinese" tag="a">
                Geometry Dash Chinese
              </n-button>

              <n-button href="https://github.com/WEGFan/Geometry-Dash-Savefile-Fix" tag="a">
                存档修复器
              </n-button>
            </n-space>
          </n-card>
        </div>
      </n-layout-content>

      <n-layout-footer class="text-center p-2.5 lg:p-5" position="absolute">
        <n-text>&copy; 2022 - {{ new Date().getFullYear() }}</n-text>
        <n-divider vertical/>
        <n-button href="/" tag="a" text>几何冲刺玩家网</n-button>
        <n-divider vertical/>
        <n-button href="https://beian.miit.gov.cn" tag="a" text>吉ICP备18006293号</n-button>
      </n-layout-footer>
    </n-layout>
  </n-config-provider>
</template>
