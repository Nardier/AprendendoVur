<template>
  <div id="app">
    <button @click="disconnect" v-if="status === 'connected'">Disconnect</button>
    <button @click="connect" v-if="status === 'disconnected'">Connect</button> {{ status }}
    <br />
    <div>
      <GChart
        type="LineChart"
        :data="chartLineData"
        :options="chartLineOptions"
      />
    </div>
        <div>
      <GChart
        type="BarChart"
        :data="chartBarData"
        :options="chartBarOptions"
      />
    </div>
    <br />  
  </div>
</template>

<script>

import Vue from 'vue'
import Grafico from './Grafico'
import { GChart } from 'vue-google-charts'


export default {
  name: 'Home',
  components:{
    Grafico,
    GChart

  },
  data () {
    return {
      message: "",
      logs: [],
      status: "disconnected",
      chartLineData: [
        ["Tempo", "Resposta"],
        ["", 0],
      ],
      chartLineOptions: {   
          curveType: 'function',
          legend: { position: 'bottom' },
          title: 'Teste Latencia',
        
      },
      chartBarData: [
        ["Tempo", "Codigo"],
        ["200", 55],
        ["207", 2],
      ],
      chartBarOptions: {   
          curveType: 'function',
          orientation: 'horizontal',
          legend: { position: 'bottom' },
          title: 'Status Code',
        
      }
    }
    
  },
  ResponseStatsModel(){
    Response_Body
    Response_Time
    Response_StatusCode
    Response_Status
    TEOM
    Request_Url
    },

  methods: {
    connect() {
      this.socket = new WebSocket("ws://localhost:8081/stats");
      this.socket.onopen = () => {
        this.status = "connected";
        this.logs.push({ event: "Connected to", data: 'wss://localhost:8080'})
        this.socket.onmessage = ({data}) => {  
          
          var parsed = JSON.parse(data)

          this.chartBarData.forEach((element) => {
            element[0] 
            
          });

          if(this.chartLineData.length <= 10 ){
            this.chartLineData.push([parsed.TEOM + ' sec' , parsed.Response_Time]);            
          }else{
            this.chartLineData.splice(1,1)
            this.chartLineData.push([parsed.TEOM + ' sec' , parsed.Response_Time])
          }
        
        };
      };
    },  
    disconnect() {
      this.socket.close();
      this.status = "disconnected";
      this.logs = [];
    },
    sendMessage(e) {
      this.socket.send(this.message);
      this.logs.push({ event: "Sent message", data: this.message });
      this.message = "";
    }
  }
}
</script>
<style scoped>

</style>