<template>
   <div class="q-pa-md">
    <q-card 
    v-for="(link, index) in info"
    :key="link+index"
    v-bind="link"
    class="my-card">
      <q-video :ratio="16/9" v-bind:src="link.link" />

      <q-card-section>
        <div class="text-h6">{{link.titulo}}</div>
        <div class="text-subtitle2">Autor: {{link.autor}}</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
       {{link.descricao}}
      </q-card-section>
    </q-card>
  </div>
</template>

<script>
export default {
  name: 'multimedia',
   data ( ) {
    return {
      info: [{
        titulo: "Coronavírus: O que a covid-19 faz com o seu corpo",
        autor: "BBC News Brasil",
        descricao: "O coronavírus se propaga quando a gente inala gotículas que uma pessoa infectada no espirro ou na tosse.",
        link: "https://www.youtube.com/embed/WEMpIQ30srI"
      },
      {
        titulo: "Brasil tem 1.128 casos confirmados e 18 mortes por Coronavírus",
        autor: "Record News",
        descricao: "Balanço do Ministério da Saúde, divulgado neste sábado (21), aponta que existem 1.128 casos de Coronavírus em todo o território nacional. Apenas o estado de Roraima não contabiliza nenhum caso da doença no país.",
        link: "https://www.youtube.com/embed/uxP6OdDzSGs"
      }],
    }
  }
}
</script>
