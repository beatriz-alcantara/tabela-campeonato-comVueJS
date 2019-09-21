<template>
  <div>
   <label for="timeCasa"> 
            {{ timeCasa }}
        </label>
        <input id="timeCasa" type="number" v-model="gCasa"/>
            X
        <input id="timeFora" type="number" v-model="gFora"/>
        <label for="timeFora">
            {{ timeFora }}
        </label>
        <button @click="fimJogo(), decisao(), sorteioTimes()"> Fim de jogo </button>
   <table>
     <tr>
        <td> Time </td>
        <td> Pontos </td>
        <td> Gols marcados </td>
        <td> Gols sofrido </td>
        <td> Saldo </td>
     </tr>

     <tr :key="time.nome" v-for="time in timesOrdenados">
       <td> {{ time.nome }} </td>
       <td> {{ time.pontos }} </td>
        <td> {{ time.golsMarcados }}</td>
       <td> {{ time.golsSofridos }} </td>
       <td> {{ time | saldo() }} </td>
     </tr>
   </table>

   <ul>
     <li :key="time.nome" v-for="time in timesLibertadores"> {{ time.nome }} </li>
   </ul>
  </div>
</template>

<script>

export default {
  name: 'Tabela',
  data () {
    return {
      timeCasa: '',
      timeFora: '',
      gCasa: 0,
      gFora: 0,
      ordem: {
        colunas: ['pontos', 'golsMarcados', 'golsSofridos'],
        orientacao: ['desc', 'desc', 'asc']
      },
      times: [
        { nome: 'Palmeiras', pontos: 0, golsMarcados: 0, golsSofridos: 0 },
        { nome: 'Corinthians', pontos: 0, golsMarcados: 0, golsSofridos: 0 },
        { nome: 'Grêmio', pontos: 0, golsMarcados: 0, golsSofridos: 0 },
        { nome: 'Internacional', pontos: 0, golsMarcados: 0, golsSofridos: 0 },
        { nome: 'Flamengo', pontos: 0, golsMarcados: 0, golsSofridos: 0 },
        { nome: 'Vasco', pontos: 0, golsMarcados: 0, golsSofridos: 0 }
      ]
    }
  },
  computed: {
    timesLibertadores () {
      return this.times.slice(0,3)
    },
    timesOrdenados () {
      return _.orderBy(this.times, this.ordem.colunas, this.ordem.orientacao)
    }
  },
  mounted () {
    this.sorteioTimes()
  },
  methods: {
    sorteioTimes () {
      this.gCasa = 0
      this.gFora = 0
      let c = Math.round(Math.random()*5)
      let f = Math.round(Math.random()*5)
      if (c !== f) {
        this.timeCasa = this.times[c].nome
        this.timeFora = this.times[f].nome 
      } else {
        f = Math.round(Math.random()*5)
        this.sorteioTimes()
      }
    },
    fimJogo () {
      this.times.forEach(obj => {
        if (obj.nome === this.timeCasa) {
          obj.golsMarcados += parseInt(this.gCasa)
          obj.golsSofridos += parseInt(this.gFora)
        }
        if (obj.nome === this.timeFora) {
          obj.golsMarcados += parseInt(this.gFora)
          obj.golsSofridos += parseInt(this.gCasa)
        }
      })
    },
    decisao () {
      let timeC = this.times.filter(objeto => {
        return objeto.nome === this.timeCasa
      })
      let timeF = this.times.filter(objeto => {
        return objeto.nome === this.timeFora
      })
      if (this.gCasa > this.gFora) {
        timeC[0].pontos += 3
      } else if (this.gFora > this.gCasa) {
        timeF[0].pontos += 3
      } else {
        timeC[0].pontos += 1
        timeF[0].pontos += 1
      }
    }
  },
  filters: {
    saldo (time) {
      return time.golsMarcados - time.golsSofridos
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  tr {
    display: block;
    border-bottom: solid;
  }

  td {
    width: 150px;
  }
</style>
