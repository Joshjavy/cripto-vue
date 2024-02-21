<script setup>
  import { ref, reactive,  onMounted } from 'vue'
  import Alerta from './components/Alerta.vue'

const monedas = ref([
      { codigo: 'USD', texto: 'Dolar de Estados Unidos'},
      { codigo: 'MXN', texto: 'Peso Mexicano'},
      { codigo: 'EUR', texto: 'Euro'},
      { codigo: 'GBP', texto: 'Libra Esterlina'},
  ])

  const criptoMonedas = ref([]);
  const error = ref('');

  const cotizar = reactive({
    moneda:'',
    criptomoneda:'',
  });



  onMounted(()=>{
    const url =('https://min-api.cryptocompare.com/data/top/totaltoptiervolfull?limit=20&tsym=USD');
    fetch(url)
      .then(respuesta=>respuesta.json())
      .then(({Data}) =>criptoMonedas.value = Data)


  })

  const cotizarCripto=()=>{
    //vallidar que cotizar este lleno
    if(Object.values(cotizar).includes('')){
      error.value='Todos los campos son obligatorios'
      return;
    }
    error.value=''
    obtenerCotizacion();


  }

  const obtenerCotizacion= async ()=>{
    const {moneda,criptomoneda} =cotizar
    const url =`https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${criptomoneda}&tsyms=${moneda}`
    
    console.log(url);
  }

</script>

<template>
  <div class="contenedor">
    <h1 class="titulo">Cotizador de <span>Criptomoedas</span></h1>

    <div class="contenido">
      <Alerta v-if="error">
        {{ error }}
      </Alerta>

      <form class="formulario"
        @submit.prevent="cotizarCripto"
      >

        <div class="campo">
          <label for="moneda">Moneda</label>
          <select id="moneda"
            v-model="cotizar.moneda"
          >
            <option value="">--- Selecciona --- </option>
            <option 
              v-for="moneda in monedas"
              :value="moneda.codigo">
                {{ moneda.texto }}
            </option>
          </select>
        </div>

        <div class="campo">
          <label for="cripto">Criptomoneda</label>
          <select id="cripto"
          v-model="cotizar.criptomoneda"
          >
            <option value="">--- Selecciona --- </option>
            <option 
              v-for="criptomoneda in criptoMonedas"
              :value="criptomoneda.CoinInfo.Name">
                {{ criptomoneda.CoinInfo.FullName}}
            </option>
          </select>
        </div>

        <input type="submit" value="Cotizar"/>
      </form>
    </div>


  </div>
</template>

<style scoped>

</style>
