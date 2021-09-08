

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
        <span class="entry-daytime"><strong class="entry-daytime">{{entry[0]}}Uhr, {{entry[1].replace("/", ".")}} </strong></span>
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
      gsheet_url: "https://sheets.googleapis.com/v4/spreadsheets/19UVN4MxwzQAf4Hg4f_4vyl8ayttwMOTZoEzPLgtzlzQ/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyD2fAjflA1WgD32AcrmyPxzhh7lNmwarD4",
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
/* #app { 

} */
#title{
  font-family: 'Inter', sans-serif;
  color: #2c3e50;
  font-size: 7vw;
  margin: 1vw;
  padding: 2.5vw;
}
#currentDates{
  width: 3.35vw;
  height: 7.5vw;
  padding: 1vw;
  padding-left: 5vw;
  font-size: 6vw;
  font-family: Inter;
  opacity: 0.5;
  color:black;
}
#entry-body{
padding: 1vw;  
}
.entry-title{
  font-family: 'Inter', sans-serif;
  background: #0F05A0;
  color: rgb(245, 210, 157);
  margin-top: 2vw;
  margin-bottom: 0;
  font-size: 4vw;
}
.entry-list{
  padding: 2vw;
  margin: 2.5vw 0;
  background: #0f05a0;
  font-size: 3vw;
  line-height: 1.3;
  font-family: 'Inter', sans-serif;
  

}
.entry-daytime{
  background: #0F05A0;
  color: red;
  font-family: 'Inter', sans-serif;
  font-size: 4vw;
}
.entry-description{
  background: #0F05A0;
  font-size: 3.5vw;
}
#body-list{
  padding: 0;
  font-style: normal;
  font-size: 2.8vw;
  color: rgb(245, 210, 157);
  list-style: none;
}
#entry-footers{ 
  left: 0;
  bottom: 0;
  width: 100vw;
  background: white;
  
  position: fixed;
  clear:both;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-around;
  height: 10vw;
}
/*Images*/
#Opportunity{
  width: 29.6vw;
  height: 5.5vw;
}
#SAG{
  width: 27.3vw;
  height: 5.2vw;
}
#STZH{
  width: 23vw;
  height: 4.4vw;
}
</style>
