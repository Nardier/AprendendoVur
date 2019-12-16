<template>
    <div class="grafico">
        <v-container>
          <v-row>
            <v-btn class="ma-2" outlined color="red" @click="disconnect" v-if="status === 'connected'">Disconnect</v-btn>
            <v-btn class="ma-2" outlined color="green" @click="connect" v-if="status === 'disconnected'">Connect</v-btn>
            <div v-if="status === 'disconnected'">
            <v-chip class="ma-2" color="red" text-color="white">
                Off
                <v-icon right>mdi-monitor</v-icon>
            </v-chip>
        </div>
        <div v-else>
            <v-chip class="ma-2" color="green" text-color="white">
                On
                <v-icon right>mdi-monitor</v-icon>
            </v-chip>
        </div>
          </v-row>
        </v-container>
        <br />
        <v-container>
            <v-col>
              <v-icon class="pr-2" >mdi-chart-histogram</v-icon>
                <h3>Monitorando: {{ url }}</h3>
            </v-col>
        </v-container>

        <div>
            <GChart type="AreaChart" :data="chartLineData" :options="chartLineOptions" />
        </div>
        <div>
            <GChart type="PieChart" :data="chartPieData" :options="chartPieOptions" />
        </div>
        <br />
        <!-- <v-simple-table>
            <template v-slot:default>
                <thead>
                    <tr>
                        <th class="text-left">Name</th>
                        <th class="text-left">Calories</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in desserts" :key="item.name">
                        <td>{{ item.name }}</td>
                        <td>{{ item.calories }}</td>
                    </tr>
                </tbody>
            </template>
        </v-simple-table> -->
    </div>
</template>

<script>
import Vue from 'vue'
import { GChart } from 'vue-google-charts'
//TODO: Fazer separação dos componentes
export default {
  name: 'grafico',
  components: {  
      GChart  
  },
  data () {
    return {
      message: "",
      contador: 0,
      logs: [],
      status: "disconnected",
      chartLineData: [
        ["Tempo", "Resposta"],
        ["", 0],
      ],
      chartLineOptions: {   
          curveType: 'function',
          legend: { position: 'bottom', textStyle: {color: 'white'} },
          title: 'Teste Latencia',
           titleTextStyle: {
           color: '#FFFFFF'
           },
           colors: ['#1FA641'],
          backgroundColor: '#393D40',
        
      },
      chartPieData: [
        ["Tempo", "Codigo"],
        ["", 1],
      ],
      chartPieOptions: {   
          curveType: 'function',
          orientation: 'horizontal',
          legend: { position: 'bottom', textStyle: {color: 'white'} },
          colors: ['#BDBDBF', '#315D40'],
          title: 'Status Code',
           titleTextStyle: {
           color: 'transparent'
           },
           pieHole: 0.35,
          pieSliceTextStyle: {
            color: 'white',
          },
          backgroundColor: 'transparent',
        
      },      
      url:""
    }
  },

  methods: {
    connect() {
      this.socket = new WebSocket("ws://localhost:8081/stats");
      this.socket.onopen = () => {
        this.status = "connected";
        this.logs.push({ event: "Connected to", data: 'wss://localhost:8080'})
        this.socket.onmessage = ({data}) => {  
          var parsed = JSON.parse(data)
          this.contador++          

          this.url = parsed.Request_Url

          var element = this.chartPieData.findIndex(element => {
            if(element[0] === parsed.Response_StatusCode){
              return element
            }
          });
          console.log(parsed)
          
          if(element <= 0){
            this.chartPieData.push([parsed.Response_StatusCode,1])
          }else{
            if(this.chartPieData.length > 5){
               this.chartPieData.splice(1,1) 
            }
            this.chartPieData[element][1]++
          }

          if(this.chartLineData.length <= 10 ){
            this.chartLineData.push([parsed.TEOM + ' s' , parsed.Response_Time]);            
          }else{
            this.chartLineData.splice(1,1)
            this.chartLineData.push([parsed.TEOM + ' s' , parsed.Response_Time])
          }

        };
      };
    },  
    disconnect() {
      this.socket.close();
      this.status = "disconnected";
      this.logs = [];
    }
  }
}
</script>
