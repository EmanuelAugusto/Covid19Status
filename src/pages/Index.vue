<template>
  <q-page class="flex flex-center">

 <div class="q-pa-md">
      <q-input square standout bottom-slots v-model="text" label="Pesquisar localidade">
        <template v-slot:prepend>
          <q-icon name="close" @click="loadData" />
        </template>
        <template v-slot:append>
          <q-icon name="search" @click="findCountry" class="cursor-pointer" />
        </template>
      </q-input>


    <p><q-icon size="md" name="autorenew" @click="loadData" class="cursor-pointer" /><strong>Ultima atualização: {{lastUpdate}}</strong></p>

   

    <q-card dark bordered class="bg-grey-100 my-card"
     v-for="(link, index) in info"
     :key="link+index"
     v-bind="link">

    <div class="row">

      <div class="col">

        <q-card-section>
        <div class="text-h6">País: {{ link.country }}</div>
        <div class="text-subtitle2">Casos confirmados: {{ link.confirmed }}</div>
        <div class="text-subtitle2">Mortes: {{ link.deaths }}</div>
        <div class="text-subtitle2">Curados: {{ link.recovered }}</div>
      </q-card-section>
      
      </div>
  
    </div>
      

    </q-card>

 </div>


  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
   data ( ) {
    return {
      info: null,
      backup: null,
      nextUrl: "https://covid-19-coronavirus-statistics.p.rapidapi.com/v1/stats",
      text: '',
      lastUpdate: null
    }
  },
  methods: {
  loadData () {
    this.text = '';
    this.$axios.get(this.nextUrl, 
    { headers: 
    { "x-rapidapi-host": "covid-19-coronavirus-statistics.p.rapidapi.com",
	    "x-rapidapi-key": "460bc27988msh7658cb742578d74p15c818jsn789899e2f487"} 
    })
      .then((response) => {
        this.info = response.data.data.covid19Stats
        this.backup = response.data.data.covid19Stats
        console.log(response.data)

        let dataAtt = response.data.data.lastChecked.split("-")
        let dataAtt2= dataAtt[2].split("T");

        this.lastUpdate = dataAtt2[0]+"/"+dataAtt[1]+"/"+dataAtt[0]

        this.$q.notify({
           type: 'warning',
           message: `Versão Beta`,
           position: 'top',
        })

      })
      .catch(() => {
        this.$q.notify({
          color: 'negative',
          position: 'top',
          message: 'Falha no Carregamento',
          icon: 'report_problem'
        })
      })
  },
  findCountry () {
    const result = this.info.find( country => country.country === this.text);

    if(result == null){
       console.log('Não encontrado')
        this.$q.notify({
          color: 'negative',
          position: 'top',
          message: 'País não encontrado',
          icon: 'report_problem'
        })
       
    }else{
        this.info = [{
          country: result.country,
          confirmed: result.confirmed,
          deaths: result.deaths,
          recovered: result.recovered
        }]
        this.$q.notify({
           type: 'warning',
           message: `País encontrado`,
           position: 'top',
        })
    }
  }
},
beforeMount(){
 this.loadData();
}
}
</script>
