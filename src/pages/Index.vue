<template>
  <q-page class="flex flex-center">
<p><strong>Ultima atualização:   {{lastUpdate}}</strong></p>
 <div class="q-pa-md">
      <q-input square standout bottom-slots v-model="text" label="Pesquisar localidade">
        <template v-slot:prepend>
          <q-icon name="close" @click="loadData" />
        </template>
        <template v-slot:append>
          <q-icon name="search" @click="findCountry" class="cursor-pointer" />
        </template>
      </q-input>

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
        <div class="text-subtitle2">Suspeitos: {{ link.recovered }}</div>
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
      nextUrl: " https://covid-19-coronavirus-statistics.p.rapidapi.com/v1/stats",
      text: '',
      lastUpdate: null
    }
  },
  methods: {
  loadData () {
    this.text = '';
    this.$axios.get(this.nextUrl, { headers: 
    { "x-rapidapi-host": "covid-19-coronavirus-statistics.p.rapidapi.com",
	    "x-rapidapi-key": "460bc27988msh7658cb742578d74p15c818jsn789899e2f487" } 
    })
      .then((response) => {
        this.info = response.data.data.covid19Stats
        this.backup = response.data.data.covid19Stats
        this.lastUpdate = response.data.data.lastChecked
        console.log(response.data.data)
        //this.nextUrl = response.data.next
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
    console.log(this.text)
    this.info = this.backup
   for (let i = 0; i < this.info.length; i++) {
    // console.log(this.info[i].country)
     if(this.info[i].country == this.text){
        this.info = [{
          country: this.info[i].country,
          confirmed: this.info[i].confirmed,
          deaths: this.info[i].deaths,
          recovered: this.info[i].recovered
        }]
     }else{
      // this.$q.notify({
      //    color: 'negative',
      //    position: 'top',
      //    message: 'País não encontrado',
      //    icon: 'report_problem'
      //  })
     }
   }

   console.log(this.info)
  }
},
beforeMount(){
 this.loadData();
}
}
</script>
