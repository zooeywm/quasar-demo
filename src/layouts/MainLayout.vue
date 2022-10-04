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
        <q-toolbar-title> 记账本 </q-toolbar-title>

        <div>版本：v1.0.0</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" bordered>
      <q-list>
        <q-item-label header> 应用介绍 </q-item-label>
        <EssentialInfo
          v-for="link in essentialInfos"
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

<script setup lang="ts">
import { ref } from 'vue';
import EssentialInfo, {
  EssentialInfoProps,
} from 'components/EssentialInfo.vue';
import { uid } from 'quasar';

const essentialInfos: EssentialInfoProps[] = [
  {
    id: uid(),
    title: '作者',
    caption: 'zooeywm',
    icon: 'face',
  },
];

const leftDrawerOpen = ref(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>
