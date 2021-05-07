<template>
  <div>
    <div class="calendar">
      <v-row justify="center">
        <v-date-picker
            color="primary lighten-1"
            header-color="black"
            v-model="calendarDate"
            first-day-of-week=1
            elevation="10"
            :range="isRange"
            @change="changeCalendar">
        </v-date-picker>
      </v-row>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "TestComponent",
  props: ['date', 'format'],
  data(){
    return{
      isRange: this.date instanceof Array,
      DATE: this.date,
      FORMAT: this.format,
      calendarDate: null,
      currentDate: null,
    }
  },
  watch: {
    date(){
      this.DATE = this.date
      this.toCalendarFormat()
    }
  },
  methods:{
    changeCalendar(){
      this.currentDate = this.calendarDate
      this.toInputFormat()
    },
    checkValid(value){
      if(this.isRange){
        this.calendarDate = this.currentDate

        let dateFrom = moment(value[0])
        let dateTo = moment(value[1])

        if(!dateFrom.isValid()) {
          this.calendarDate[0] = moment().format('YYYY-MM-DD')
          this.DATE[0] = moment().format(this.FORMAT)
        }
        if(!dateTo.isValid()){
          this.calendarDate[1] = moment().format('YYYY-MM-DD')
          this.DATE[1] = moment().format(this.FORMAT)
        }
        return ''
      }
      else {
        this.calendarDate = this.currentDate
        let date = moment(value);
        if(!date.isValid()) this.calendarDate = moment().format('YYYY-MM-DD')
      }
    },
    toCalendarFormat(){
      if(this.isRange){
        this.currentDate = []
        this.currentDate[0] = moment(this.DATE[0], this.FORMAT).format("YYYY-MM-DD")
        this.currentDate[1] = moment(this.DATE[1], this.FORMAT).format("YYYY-MM-DD")
      }
      else {
        this.currentDate =  moment(this.DATE, this.FORMAT).format("YYYY-MM-DD")
      }

      this.checkValid(this.currentDate)

      this.$emit('setDate', this.DATE)
    },

    toInputFormat(){
      if(this.isRange){
        this.DATE = []
        this.DATE[0] =  moment(this.currentDate[0], 'YYYY-MM-DD').format(this.FORMAT)
        this.DATE[1] =  moment(this.currentDate[1], 'YYYY-MM-DD').format(this.FORMAT)
      }
      else {
        this.DATE = null
        this.DATE =  moment(this.currentDate, 'YYYY-MM-DD').format(this.FORMAT)
      }
      this.$emit('setDate', this.DATE)
    },
  },
  created() {
    this.toCalendarFormat()
  },
}
</script>

<style scoped>

</style>