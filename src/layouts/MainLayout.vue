<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-black text-white">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Covid-19
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Menu
        </q-item-label>
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
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: [
         {
          title: 'Home',
          caption: 'Painel Global',
          icon: 'language',
          link: '/'
        },
        {
          title: 'Ministério da Saúde',
          caption: 'Dicas de prevenção',
          icon: 'account_balance',
          link: 'https://coronavirus.saude.gov.br/'
        },
        {
          title: 'Dados do Brasil',
          caption: 'Mais detalhes sobre a expansão do Covid-19 no Brasil',
          icon: 'timeline',
          link: '/#/dados'
        },
         {
          title: 'Corona Vírus TV',
          caption: 'Vídeos explicativos',
          icon: 'live_tv',
          link: '/#/videos'
        }
      ]
    }
  }
}
</script>
