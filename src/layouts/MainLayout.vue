<!--
 * @Date           : 2022-03-10 18:36:58
 * @FilePath       : /jinnian-cms-admin/src/layouts/MainLayout.vue
 * @Description    :
-->
<template>
  <q-layout view="hHh Lpr lFf">
    <q-resize-observer @resize="onResize" />
    <q-header elevated>
      <q-toolbar>
        <q-btn flat dense round @click="toggleLeftDrawer" icon="menu" aria-label="Menu" />
        <q-toolbar-title>
          {{ title }}
        </q-toolbar-title>
        <q-space />
        <div class="q-gutter-sm row items-center no-wrap">
          <q-btn round dense flat color="white" :icon="$q.fullscreen.isActive ? 'fullscreen_exit' : 'fullscreen'"
            @click="$q.fullscreen.toggle()" v-if="$q.screen.gt.sm">
          </q-btn>

          <q-btn round dense flat color="white" icon="notifications">
            <q-badge color="red" text-color="white" floating> 5 </q-badge>
            <q-menu>
              <q-list style="min-width: 100px">
                <messages></messages>
                <q-card class="text-center no-shadow no-border">
                  <q-btn label="View All" style="max-width: 120px !important" flat dense class="text-indigo-8"></q-btn>
                </q-card>
              </q-list>
            </q-menu>
          </q-btn>
          <q-btn round flat>


            <q-btn-dropdown color="primary">



              <template v-slot:label>
                <q-avatar size="26px">
                  <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
                </q-avatar>
              </template>
              <q-list>
                <q-item clickable v-close-popup @click="tuichu_denglu">
                  <q-item-section>
                    <q-item-label>退出登录</q-item-label>
                  </q-item-section>
                </q-item>


              </q-list>
            </q-btn-dropdown>


          </q-btn>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered :width="200" class="bg-primary text-white">
      <q-list>
        <q-item v-for="(item1, index1) in menu_config" :key="index1" clickable @click="handle_menu_item_click(item1)"
          class="cursor-pointer" :active="current_route_name == item1.name" active-class="q-item-no-link-highlighting">
          <q-item-section avatar>
            <q-icon name="dashboard" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ item1.label }}</q-item-label>
          </q-item-section>
        </q-item>


      </q-list>
    </q-drawer>

    <q-page-container class="bg-grey-2">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from "components/EssentialLink.vue";
import Messages from "./Messages";

import { defineComponent, ref } from "vue";
import {   SessionStorage,Notify } from 'quasar'
import menuConfig from "src/config/menu.js";
import { mapActions } from "vuex";
export default defineComponent({
  name: "MainLayout",

  components: {
    EssentialLink,
    Messages,
  },
  data() {
    return {
      title: process.env.title,
    };
  },

  setup() {
    const leftDrawerOpen = ref(false);
    const menu_config = ref(menuConfig);

    return {
      leftDrawerOpen,
      menu_config,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
  computed: {
    current_route_name() {
      return this.$route.name;
    },
  },
  methods: {
    ...mapActions([
      "set_window_size", //also supports payload `this.nameOfAction(amount)`
    ]),
    handle_menu_item_click(item) {
      console.log('item-----', item);
      this.$router.push({
        name: item.name,
      });
    },
    onResize(size) {
      console.log(size);
      this.set_window_size(size);
    },
    tuichu_denglu(){
      SessionStorage.remove('token')
      this.$router.push({name:"login"})
    }
  },
});
</script>
