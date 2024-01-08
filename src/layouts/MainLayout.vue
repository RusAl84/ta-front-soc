<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Меню"
          @click="toggleLeftDrawer"
        />
        <q-toolbar-title>
          Информационно-аналитическая система противодействия социальному
          инжинирингу на основе методов интеллектуального анализа текстов
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Ссылки </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
  {
    title: "Загрузка данных",
    icon: "code",
    link: "/load",
  },
  {
    title: "БД социнжиниринга",
    icon: "school",
    link: "/key",
  },
  {
    title: "Поиск социнжиниринга",
    icon: "record_voice_over",
    link: "/find",
  },
  {
    title: "О проекте",
    icon: "rss_feed",
    link: "/about",
  },
];

export default defineComponent({
  name: "MainLayout",

  components: {
    EssentialLink,
  },

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>
