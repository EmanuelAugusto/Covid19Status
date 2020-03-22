<template>
  <q-page class="flex flex-center">

    <div class="q-pa-md">



        <h4>Dados Abertos</h4>



    <q-card class="my-card">
      <img src="https://cdn.shopify.com/s/files/1/0508/9281/products/mm_ss_framed-canvas_brazil_SS_flags_36x24_black-white_mockup_Wall-Horizontal_24x36_332b7b65-11a0-4ef3-9fd5-f40ebb45c498_grande.png?v=1491940018">

      <q-card-section>
        <div class="text-h6">{{info.country}}</div>
        <div class="text-subtitle2">CONFIRMADOS: {{info.confirmed}}</div>
         <div class="text-subtitle2">MORTES: {{info.deaths}}</div>
          <div class="text-subtitle2">CURADOS: {{info.recovered}}</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
      
      </q-card-section>
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
    {  "x-rapidapi-host": "covid-19-coronavirus-statistics.p.rapidapi.com",
	     "x-rapidapi-key": "460bc27988msh7658cb742578d74p15c818jsn789899e2f487"} 
    })
      .then((response) => {
      console.log(response)
      this.info = response.data.data.covid19Stats
      const result = this.info.find( country => country.country === "Brazil");
     // console.log(result)
      let infoformated = [{
          country: result.country,
          confirmed: result.confirmed,
          deaths: result.deaths,
          recovered: result.recovered
      }]

      this.info = infoformated[0];
      console.log(this.info)

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
