<template>
    <div class="carte">
    
    <b-card :title="city.name" :class="color">
           
    <b-card-text v-if="!loading">
      Qualité de l'air : {{ city.iqa }}
      <b-button @click="deleteCityAction" class="buttonClose">Supprimer</b-button>  
    </b-card-text>
    
    <b-spinner v-if="loading" variant="primary" label="Spinning"></b-spinner>
  </b-card>
    </div>
</template>

<script>
import {AirQualityService} from "@/services/AirQuality.service"

    export default {
        props: {
              city: {
                type: Object,
                default: null
            }
        },
        data() {
            return {
                color: "",
                loading:false
            };
        },
        async mounted () {
            this.loading=true;
            const infosCity = await AirQualityService.getAirQuality(this.city.name);
            this.loading = false ;

            this.city.iqa= infosCity.aqi;

            if(this.city.iqa<=30)this.color="pollution-faible"
            if(this.city.iqa>30 && this.city.iqa<=50)this.color="pollution-medium"
            if(this.city.iqa>50)this.color="pollution-forte"
        },
        methods: {
                deleteCityAction() {
                    this.$emit("deleteCity" , this.city)
            }
        },
    };
</script>

<style lang="scss" scoped>
.pollution-faible{
    background: green;
    color: white;
}

.pollution-medium{
    background: rgb(238, 160, 16);
    color: white;
}

.pollution-forte{
    background: rgb(194, 13, 13);
    color: white;
}

.carte {
    margin: 5px;
}

.buttonClose{
    float:right;
    border: solid black 1px;
}


</style>