<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title> Vue Task </q-toolbar-title>

        <div>My Task V-1.0.0</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Essential Links </q-item-label>

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
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
  {
    title: "Educations & CV",
    caption: "Green University of Bangladesh --    BSC in CSE (2018-2022)",
    icon: "school",
    link: "https://drive.google.com/file/d/1I-fWsqrag8q2FUqDbuh5qwAsz9yrUKoO/view?usp=sharing",
  },
  {
    title: "Github",
    caption: "github.com/ShuvobrotoBiswas",
    icon: "code",
    link: "https://github.com/ShuvobrotoBiswas",
  },

  {
    title: "Facebook",
    caption: "facebook/Shuvobroto(Rocky)",
    icon: "facebook",
    link: "https://www.facebook.com/Coder.Rocky.Samrat.Devils.King.Software.Developer/",
  },
  {
    title: "Twitter",
    caption: "twitter.com/ShuvobrotoB",
    icon: "person",
    link: "https://twitter.com/ShuvobrotoB",
  },
  {
    title: "Linkedin",
    caption: "Linkedin/Shuvobroto",
    icon: "task_alt",
    link: "https://www.linkedin.com/in/shuvobroto-biswas-58a045193/",
  },
];

import { defineComponent, ref } from "vue";

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
