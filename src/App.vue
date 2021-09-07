

<template>
  <div id="app">
  <!-- Hier codes Du HTML-->
  
  <header id="title">
    {{title}}
  </header>
      <div id="currentDates">  
        {{currentDate}}
      </div>
   <div id="entry-body">
      <ul id="body-list">
        <li class="entry-list" v-for="entry in filteredEntries" :key="entry.id">
        <span class="entry-daytime">{{entry[0]}}Uhr, {{entry[1].replace("/", ".")}} </span>
        <h3 class="entry-title">{{entry[2]}}</h3>
        <span class="entry-description">{{entry[3]}}</span>
        </li>
      </ul>
    </div>
    <div id="entry-footers">
        <img id="STZH" src="./assets/STZH_SEB_Logo.jpg">
          <!-- alt="Opportunity Logo -->
        <img id="SAG" src="./assets/SAG_Logo_De.jpg">
          <!-- alt="SAG Logo" -->
        <img id="Opportunity" src="./assets/Opportunity.jpg">
          <!-- alt=" Stadt Zürich logo" -->  
    </div> 
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: 'app',
  data() {
    return {
      gsheet_url: "https://sheets.googleapis.com/v4/spreadsheets/1qLZJwuNv3QmwGhSj1wZZbuXNOkDKN-Ha7fo0Ca_uVVU/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyBesotaNgSaTUIhrSKjEaExdi-ksKInhoE",
      title: "Welcome to Opportunity",
      entries: [],
      currentDate:"",
     }
    },   
  /*Function*/
 computed: { 
   //computet properties are like data properties, but with a methods combined
    filteredEntries(){
      return [...this.entries].slice(1);
    }
  },
  methods: {
      getData(){
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values; 
        console.log(response);        
      });
    },
    upbtadeCurrentdate() {
      let today = new Date();
      const date = `${today.getDate()}.${
        today.getMonth() + 1
      }.${today.getFullYear()}`;
      this.currentDate = date;
      },
    refreshData() {
      this.getData();
      this.upbtadeCurrentdate();
    },
  },
  mounted(){
    this.refreshData();
    setInterval(() => {
      this.refreshData();
    },1800000);
  },
}
</script> 
<style>

* {
  background: #e8eff4;
}
#app { 

}
#title{
  font-family: 'Inter', sans-serif;
  color: #2c3e50;
  font-size: 62px;
  margin: 10px;
  padding: 25px;
}
#currentDates{
  width: 335px;
  height: 75px;
  padding: 10px;
  padding-left: 50px;
  font-size: 50px;
  font-family: Inter;
  opacity: 0.5;
  color:black;
}
#entry-body{
padding: 10px;  
}
.entry-title{
  font-family: 'Inter', sans-serif;
  background: #0F05A0;
  color: rgb(245, 210, 157);
}
.entry-list{
  padding: 35px 40px;
  margin: 25px 0;
  background: #0f05a0;
  font-size: 28px;
  line-height: 1.3;
}
.entry-daytime{
  background: #0F05A0;
  color: red;
  font-family: 'Inter', sans-serif;
}
.entry-description{
  background: #0F05A0;
}
#body-list{
  font-style: normal;
  font-size: 28px;
  color: rgb(245, 210, 157);
  list-style: none;
}
#entry-footers{ 
  left: 0;
  bottom: 0;
  width: 100%;
  background: white;
  position: fixed;
  clear:both;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  height: 50px;
}
/*Images*/
#Opportunity{
  width: 296px;
  height: 55px;
}
#SAG{
  width: 273px;
  height: 52px;
}
#STZH{
  width: 230px;
  height: 44px;
}
</style>
