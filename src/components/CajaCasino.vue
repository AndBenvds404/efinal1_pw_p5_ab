<template>
    <div class="container">
        
        <img class="img-caja" :src="urlImg" alt="">
        <label for="">{{ respuesta }}</label>

    </div>
</template>

<script>
export default {
    data(){
        return{

        }
    },

    props:{
        respuesta:{
            default:'xxxxxxx'
        },
        urlImg:{
            type: String
        }
    }

}
</script>

<style scoped>

.container{
    display: grid;
   justify-content: center;
   align-items: center;
   margin: 0px 37px
}


.img-caja{
    width: 200px;
    height: 300px;
}


label{
    font-size: 27px;
}
</style>