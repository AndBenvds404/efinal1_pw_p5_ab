<template>
    <div class="container">

    <div class="juego" v-if="mostrarjuego">
        <h1>Juego Casino</h1>
            <div>
                    <label for="">puntaje: {{ puntaje }}</label>
                    <label for="">intentos: {{ intentos }}</label>
            </div>

            <div class="cuadrosImg">
                <cajaImagen :urlImg='listaUrlImg[2]' :respuesta="listaRespuestas[2]"></cajaImagen>
                <cajaImagen :urlImg="listaUrlImg[1]"  :respuesta="listaRespuestas[1]"></cajaImagen>
                <cajaImagen :urlImg="listaUrlImg[0]" :respuesta="listaRespuestas[0]"></cajaImagen>
            </div>

            <button @click="consultarApi()"> JUGAR</button>
    </div>

    <div class="mensajeWin"  v-if="mostrarwin">
        <h1> Puntaje: {{ puntaje }}</h1>
        <h1>{{ MensajeWin }}</h1>
        <button @click="reiniciar()" > Reiniciar</button>

    </div>

    <div  class="mensajeLose" v-if="mostrarlose">
        <h1>has utilizado tus 5 intentos</h1>
        <h1>{{ mensajeLose }}</h1>
        <button @click="reiniciar()" > Reiniciar</button>

    </div>

        
        
    </div>
</template>

<script>
import cajaImagen from './CajaCasino.vue'
export default {
    
    components: {
        cajaImagen
  },

  data(){
    return{
        puntaje:0,
        intentos:0,
        listaUrlImg:["https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
                 "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
                    "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg"],
                    
        listaRespuestas:['xxxxx','xxxxx','xxxxx'],
        mensajeLose:"El juego ha termindo, intentalo nuevamente",
        MensajeWin:"Felicitaciones has ganado un premio de $10.000,00",
        mostrarjuego:true,
        mostrarlose:false,
        mostrarwin:false

    }
  },

  methods:{
    async consultarApi(){
        let listAuxResp=[]
        let listAuxImg=[]
        for (var i = 0 ; i < 3; i++) {
            let {answer, image} = await fetch('https://yesno.wtf/api').then(r=>r.json())
            listAuxResp.push(answer);
            listAuxImg.push(image)
        }
        this.listaRespuestas = listAuxResp;
        this.listaUrlImg = listAuxImg;
        this.logicaJuego()
        
        console.log(this.listaRespuestas);
        console.log(this.listaUrlImg);

    },

    logicaJuego(){
        
        if(this.intentos <5 || this.puntaje>=10){
            
            if(this.puntaje >=10){
                this.mostrarjuego=false
                this.mostrarwin=true
                

            }else{

                var contadorYes = 0;
                for (var elemento of this.listaRespuestas) {
                    if (elemento === 'yes') {
                        contadorYes++;
                    }
                }

                if (contadorYes === 1){
                    this.puntaje = this.puntaje+1
                }else if(contadorYes===2){
                    this.puntaje = this.puntaje+2
                }else if(contadorYes===3){
                    this.puntaje = this.puntaje+5

                }
                this.intentos++;
            }

        }else{
            
            this.mostrarjuego = false
            this.mostrarlose=true
           


        }


    },

    reiniciar(){

        this.puntaje = 0
        this.intentos = 0
        this.listaUrlImg=["https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
                 "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
                    "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg"]
        
                    
        this.listaRespuestas=['xxxxx','xxxxx','xxxxx']
        this.mostrarjuego=true
        this.mostrarwin=false
        this.mostrarlose=false

    }


  }


}
</script>

<style scoped>

.cuadrosImg{
    display: flex;
    justify-content: center;
    align-items: center;
}


.mensajeWin{
    color: blue ;
}

.mensajeLose{
    color: red;

}

label{
    margin: 0px 30px;
    font-size: 37px;
}

button{
    padding: 10px;
    width: 140px;
    font-size: 27px;
    border: solid 3px black;
}
</style>