

<template>




        <rect  v-for="day in filteredDays" :key=" week * 7 + day "  :width="SQUARE_SIZE"  :height="SQUARE_SIZE" :x="(week+2)*16" :y="(day+1)*16" 
                rx="2" ry="2"
              :fill="handleColor(week*7+day)" 
              @click="handleClick(week*7+day)"
        >
                 <title>{{ getTitle(week*7+day) }}</title>

        </rect>
        
        
  
</template>

<script>

import { DAYS_IN_WEEK,MILLISECONDS_IN_ONE_DAY } from "../constants"



export default {
 name:"Week",
 props:['week',"startDate",'endDate',"range",'values','getStartDateWithEmptyDays','showchange'],

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

        handleClick(index){

                this.showchange.length=0;

               const count = this.mapobj[index].value.count;
               const date = this.mapobj[index].value.date;

                let d = date.getDate();
                let month = date.getMonth()+1;
                const year = date.getFullYear();

                if(d<=9){
                        d ='0'+d;
                }

                if(month<=9){
                        month = '0'+month;
                }

                console.log(d+"-"+month+"-"+year);

                const string = d+"-"+month+"-"+year;

               if(count===0){
                   this.showchange.push('No changes');
               }
                else{

                        fetch(`https://changes.free.beeceptor.com/changes/${string}`)
                        .then(res=> res.json())
                        .then(data =>{
                                // console.log(data);

                                data.map((item) =>{

                                        this.showchange.push(item.description);
                                })
                                
                                // console.log(this.showchange)
                        })
                        
                }
               
                
        },

        handleColor(index){

             return this.mapobj[index].color;

        },

         getColor(value){

            if(value.count === 0){
              return 'lightgrey';
            }
            else
            if(value.count >=1 && value.count <=3){
              return 'orange';
            }
            else
              if(value.count>=4 && value.count<=6){
                return 'darkorange';
              }
            else
              if(value.count>=7 && value.count <=10){
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