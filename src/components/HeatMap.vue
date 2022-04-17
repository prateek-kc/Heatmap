<template>

    <svg :viewBox="getViewBox()" >
        
        <g v-for="week in weeks" :key="week" :transform="getTransformForWeek(week)"  >
            <Week  :week="week"  :startDate="startDate" :endDate="endDate" :range="range" :values="values" 
            :getStartDateWithEmptyDays="getStartDateWithEmptyDays" />
        </g>

        <text v-for="day in DAY_LABELS" :key="day"  x="0" :y="(16 * DAY_LABELS.indexOf(day)) + (DAY_LABELS.indexOf(day)+1)+24"  style="font-size:14px" > {{day}} </text>

        <text v-for="month in MONTH_LABELS" :key="month"  :x="(16 * 4.3* (MONTH_LABELS.indexOf(month)+0.5))" y="10"  style="font-size:14px" > {{month}} </text>
        
        
    </svg>

      

</template>


<script>

import {MILLISECONDS_IN_ONE_DAY, DAYS_IN_WEEK, MONTH_LABELS, DAY_LABELS } from '../constants';
import Week from './Week.vue';



export default {
    name:'HeatMap',
    props:['startDate','endDate','counts'],
    components:{Week},
    data(){
    

      const SQUARE_SIZE = 15;
      
     
      const diffTime = Math.abs(this.endDate - this.startDate);
      const range = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
    
    

      let values = new Array(range);

      let resObj = {};
     

      for(var i=0;i<values.length;i++){

        var date = new Date(this.startDate);

        var day = i * (60 * 60 * 24 * 1000);

        var end = new Date(date.getTime() + day);

        const newDate = end.toLocaleDateString('en-CA');


        if(newDate in this.counts){

            resObj = {...resObj,date:end,count: this.counts[newDate]}

        }
        else {
          resObj = {...resObj,date:end, count:0}
        }

        values[i] = resObj;
         
      }

      console.log(values)


     const noOfWeeks = this.numberOfWeeks(range);

     const weeks = new Array(noOfWeeks);

     for(let i=0;i<weeks.length;i++){
         weeks[i] = i;
     }

     console.log(weeks);
 
      return{
        values:values,
        SQUARE_SIZE:SQUARE_SIZE,
        range:range,
        weeks:weeks,
        MONTH_LABELS:MONTH_LABELS,
        DAY_LABELS:DAY_LABELS
      }
    },

    methods:{

       shiftDate(date, numDays) {
          const newDate = new Date(date);
          newDate.setDate(newDate.getDate() + numDays);
          return newDate;
        },


         getNumEmptyDaysAtStart() {
          return this.startDate.getDay();
        },

        getNumEmptyDaysAtEnd() {
          return DAYS_IN_WEEK - 1 - this.endDate.getDay();
        },

        getStartDateWithEmptyDays() {
          return this.shiftDate(this.startDate, -this.getNumEmptyDaysAtStart());
         },

        numberOfWeeks(range){
            return Math.ceil((range + this.getNumEmptyDaysAtEnd() + this.getNumEmptyDaysAtStart())/7);
        },

        getWeekdayLabelSize() {
            // if (!this.props.showWeekdayLabels) {
            // return 0;
            // }
            // if (this.props.horizontal) {
            // return 30;
            // }
            return SQUARE_SIZE * 1.5;
        },

        getSquareSizeWithGutter() {
          return this.SQUARE_SIZE + 1;
        },

        getTransformForWeek(weekIndex) {
    
            return `translate(${(weekIndex+1) * 0.1},0)`;
       },

        getSquareCoordinates(dayIndex) {
          // if (this.props.horizontal) {
          //   return [0, dayIndex * this.getSquareSizeWithGutter()];
          // }
          return [dayIndex * this.getSquareSizeWithGutter(), 0];
        },

        getWeekWidth() {
           return DAYS_IN_WEEK * this.getSquareSizeWithGutter();
         },

        getViewBox() {

            const width = 56 * 16;
            const height = 20*16;

            console.log(width)
      
              return `0 0 ${width} ${height}`;
        },




        


    }

}


</script>


