<template>
  <div id="app">
   <div class="container">
     <h1 class="text-center">Mesure de la qualité de l'air</h1>
     <div class="row">
       <div v-for="city of cities" :key="city.index" class="col-sm-4">
       <City :city="city" @deleteCity="deleteCityAction" />
     </div>
     </div>
     <CityForm @cityAddEvent="addCityAction"/>
     <br>
     <Alert v-if="showAlert" :type="typeAlert" :message="messageAlert"/>
     <br>
   </div>
  </div>
</template>

<script>
import City from "@/components/City"
import CityForm from "@/components/CityForm"
import Alert from "@/components/Alert"

import {AirQualityService} from "@/services/AirQuality.service"

export default {
  name: 'App',
  components: {
    City,
    CityForm,
    Alert
  },
  data() {
    return {
      cities: [
        {name: "Paris", iqa:null},
        {name: "Marseille", iqa:null},
        {name:"Lyon", iqa:null},
        {name: "Toulouse", iqa:null},
        {name: "Lille", iqa:null},
        {name: "Bordeaux", iqa:null},
        {name: "Montpellier", iqa:null},
        {name: "Saint-Etienne", iqa:null},
        {name: "Nantes", iqa:null},
        {name: "Lens", iqa:null},
        {name: "Nimes", iqa:null},
        {name: "Tokyo", iqa:null},
        {name: "Madrid", iqa:null},
        {name: "Seoul", iqa:null},
        {name: "Rome", iqa:null},
        {name: "Florence", iqa:null},
        {name: "Berlin", iqa:null},
              ],
            typeAlert:"",
            messageAlert:"",
            showAlert:false
    }
  },
  methods: {
    async addCityAction(cityName) {
      const dataForNewCity=await AirQualityService.getAirQuality(cityName);

        if (dataForNewCity !== "Unknown station"){
          this.cities.push({
          name:cityName,
          iqa:null
      });

      this.typeAlert="success";
      this.messageAlert = "Ville Ajoutée !";
      this.showAlert=true;

      }else{
            this.typeAlert="warning";
            this.messageAlert = "Ville Non Disponible";
            this.showAlert=true;
      }
    },
    deleteCityAction(city){
      const indexToDelete=this.cities.findIndex(
        cityItem => cityItem.name === city.name);
        this.cities.splice(indexToDelete,1);
    }
  }
};
</script>

<style>
h1{
  padding: 5px;
}
#app{
  background-image: url(https://cdn.pixabay.com/photo/2017/01/26/06/44/sky-2009916_960_720.jpg);
  background-size: 150em;
}
</style>
