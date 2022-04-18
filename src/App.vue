


<template>



  <div class="Heatmap-container">

    <div class="heatmap-heading" > <h1> Change Heat Map  </h1></div>

    <div class="heatmap-lower" v-if="ready">  
        <HeatMap  :startDate="new Date(caldata.startDate)"  :endDate="new Date(caldata.endDate)"  :counts="caldata.counts"  :showchange="showchange" /> 
    </div>

    <div class="heatmap-bottom">

    
          <span>Less</span>

          <div class="box" style="background-color:lightgrey"></div>

          <div class="box" style="background-color:orange" ></div>

          <div class="box"   style="background-color:darkorange" ></div>

          <div class="box"  style="background-color:coral" ></div>

           <div class="box"  style="background-color:orangered" ></div>

          <span>More</span>
     

    </div>


    
  </div>


  <div class="commit-container">

          <h1>Select a box to view Changes for that date</h1>

          <div v-for="change in showchange" :key="change" class="change-list">

              {{change}}
          </div>
  </div>

</template>

<script >
import HeatMap from "./components/HeatMap.vue";


export default{

  components:{HeatMap},
  

  data(){

   let showchange = new Array();
    
    return {
      caldata:{},
      ready:false,
      showchange:showchange
    }
  },

  methods:{



    

  },
  mounted(){
    
    fetch('https://changes.free.beeceptor.com/changes')
    .then(res => res.json())
    .then(data =>{
      console.log(data);
      this.caldata= data;
      this.ready= true;
    })
  }
}
</script>


<style>

.Heatmap-container{
  display: flex;
  flex-direction: column;
  width: 80%;
  height: 350px;
  margin: 100px auto;
  /* border:solid red 2px; */
  border-radius: 20px;
  align-items: center;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
}


.heatmap-lower{
width: 90%;
/* border: solid blue 1px;; */
height: 56%;
padding: 10px;

}

.heatmap-bottom{

  display: flex;
  justify-content: flex-end;
  width: 85%;
}

.box{
  margin-left: 10px;
  margin-right: 10px;
  height: 15px;
  width: 15px;
  border-radius: 2px;
}

.commit-container{
  display: flex;
  flex-direction: column;
  width: 80%;
  height: auto;
  margin: 100px auto;
  border-radius: 20px;
  align-items: center;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
}

.change-list{
  margin:10px;
  padding: 10px;
 
}

</style>
