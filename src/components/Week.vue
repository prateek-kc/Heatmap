

<template>




        <rect  v-for="day in filteredDays" :key=" week * 7 + day "  :width="SQUARE_SIZE"  :height="SQUARE_SIZE" :x="(week+2)*16" :y="(day+1)*16" 
                rx="2" ry="2"
              :fill="handleColor(week*7+day)" 
        >
                 <title>{{ getTitle(week*7+day) }}</title>

        </rect>
        
        
  
</template>

<script>

import { DAYS_IN_WEEK,MILLISECONDS_IN_ONE_DAY } from "../constants"

export default {
 name:"Week",
 props:['week',"startDate",'endDate',"range",'values','getStartDateWithEmptyDays'],

 data(){

   const SQUARE_SIZE = 15;

  let days = new Array(DAYS_IN_WEEK);

  for(let i=0;i<days.length;i++){
          days[i] = i;
  }


     const mapobj =  this.values.reduce((acc,value) =>{

            const date = value.date;
            const index =  Math.floor((date - this.getStartDateWithEmptyDays()) / MILLISECONDS_IN_ONE_DAY);

            acc[index] = {
              value,
              color:`${this.getColor(value)}`
            }

            return acc;

            },{});
  
  
 

  return {
          days:days,
          SQUARE_SIZE:SQUARE_SIZE,
          mapobj:mapobj
          
  }

 },

 methods:{

        handleClick(day,week){
                console.log(week+' '+day);
        },

        handleColor(index){

             return this.mapobj[index].color;

        },

         getColor(value){

            if(value.count === 0){
              return 'lightgrey';
            }
            else
            if(value.count >0 && value.count <3){
              return 'orange';
            }
            else
              if(value.count>=3 && value.count<6){
                return 'darkorange';
              }
            else
              if(value.count>6 && value.count <=10){
                      return 'coral';
              }
           else
                return 'orangered';
        },

        getTitle(index){

                const obj = this.mapobj[index];

                const date = obj.value.date;
                const count = obj.value.count;

                return `${count} changes on ${date.toString().substring(0,15)}`;
                
        }

 },

 computed:{

        filteredDays(){

               return this.days.filter((day) =>{

                //       console.log(this.week + " " + day);

                        if(  ((this.week*7+day) >= this.startDate.getDay() && (this.week*7+day) < this.startDate.getDay() + this.range) ){

                                //console.log('day'+day+" " + this.week+" "+ (this.week*7+day));
                                return day.toString();
                        }
               })

               
        },

 }



}
</script>

<style>

</style>