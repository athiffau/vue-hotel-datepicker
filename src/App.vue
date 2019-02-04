<template>
  <div id="app" style="text-align: left; padding: 10vw">
    <div>

      <div class="box">
        <h3>Allow check-in and check-out on the same day</h3>
        <DatePicker
          :minNights="0"
          :textDates="[{'date':'2019-02-05','text':'<br>100€</br>'},{'date':'2019-02-06','text':'<br><strong>59€</strong></br>'}]"
        />
      </div>

      <div class="box">
        <h3>Allow selection of single day</h3>
        <DatePicker
          :singleDaySelection="true"
        />
      </div>

      <div class="box">
        <h3>Check in only on saturday and minimum stay of 10 nights</h3>
        <DatePicker
          :disabledDaysOfWeek="['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Sunday']"
          :enableCheckout="true"
          :minNights="10"
        />
      </div>

      <div class="box">
        <h3>Check in and check-out only on saturday and maximum stay of 30 nights</h3>
        <DatePicker
          :disabledDaysOfWeek="['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Sunday']"
          :enableCheckout="true"
          :allowedRanges="[7,14,21,28]"
          :maxNights="30"
        />
      </div>

      <div class="box">
        <h3>Block all dates after December 31st of the current year</h3>
        <DatePicker
          :endDate="new Date(new Date().getFullYear(), 11, 31)"
        />
      </div>

      <div class="box">
        <h3>Block all dates after September 15th</h3>
        <DatePicker
          :endDate="new Date(new Date().getFullYear(), 8, 15)"
        />
      </div>

      <div class="box">
        <h3>Block all date ranges of more than 30 days</h3>
        <DatePicker
          :maxNights="30"
          :showBottomBar="false"
          :selectForward="false"
        />
      </div>

      <div class="box">
        <h3>Minimum stay of 3 days</h3>
        <DatePicker
          :minNights="3"
          :showBottomBar="true"
        />
      </div>

      <div class="box">
        <h3>Certain dates blocked</h3>
        <DatePicker
          :disabledDates="[
              '2017-09-14',
              '2017-09-26',
          ]"
          :showCloseButton="true"
        />
      </div>

      <div class="box">
        <h3>Allow setting a default date range ( can be used to set a range from a url param )</h3>
        <DatePicker
          :startingDateValue="new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate())"
          :endingDateValue="new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate() + 5)"
        />
      </div>

      <div class="box">
        <h3>Ranges of 7, 10 or 14 days and checkin only on saturdays</h3>
        <DatePicker
          :disabledDaysOfWeek="['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Sunday']"
          :enableCheckout="true"
          :allowedRanges="[7,10,14]"
          :minNights="7"
        />
      </div>

      <div class="box">
        <h3>Custom tooltip text</h3>
        <DatePicker
          tooltipMessage="<strong style='color: red'>Enjoy</strong> your stay!"
        />
      </div>

      <div class="box">
        <h3>Show year</h3>
        <DatePicker
          :showYear="true"
        />
      </div>

      <div class="box">
        <h3>Custom date format with i18n (e.g.: pt-PT)</h3>
        <DatePicker
          :minNights="0"
          :singleDateSelection="true"
          format="MMMM D"
          :i18n="ptPT"
        />
      </div>

      <div class="box">
        <h3>Change the first day of the week to Monday</h3>
        <DatePicker
            :i18n="{ night: 'Night',
                      nights: 'Nights',
                      'day-names': ['Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun'],
                      'check-in': 'Check-in',
                      'check-out': 'Check-Out',
                      'month-names': ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
                    }"
            :firstDayOfWeek="1"
        />
      </div>

      <div class="box">
        <h3>Set checkIn value</h3>
        <DatePicker
            :i18n="{ night: 'Night',
                      nights: 'Nights',
                      'day-names': ['Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun'],
                      'check-in': 'Check-in',
                      'check-out': 'Check-Out',
                      'month-names': ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
                    }"
            :firstDayOfWeek="1"
            :checkInValue="new Date()"
        />
      </div>

      <div class="box">
        <h3>Set checkOut value</h3>
        <DatePicker
            :i18n="{ night: 'Night',
                      nights: 'Nights',
                      'day-names': ['Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun'],
                      'check-in': 'Check-in',
                      'check-out': 'Check-Out',
                      'month-names': ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
                    }"
            :firstDayOfWeek="1"
            :checkInValue="new Date()"
            :checkOutValue="new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate() + 3)"
            @check-out-selected="getFinalPrice($event)"
        />
      </div>

      <div class="box">
        <h3>Dynamic tooltips</h3>
        <DatePicker
          :showAdvancedTooltip=true
          :advancedTooltip="getPricing"
          :advancedTextDate="setTextDate"
          :tooltipMessage="setDynamicTitle"
          :hoveringTooltip=true
          :showYear=true
          @check-out-selected="getFinalPrice($event)"
        />
      </div>

    </div>
  </div>
</template>

<script>
  import DatePicker from 'components/DatePicker.vue';
  import formatMoney from 'accounting-js/lib/formatMoney';
  import toFixed from 'accounting-js/lib/toFixed';
  import unformat from 'accounting-js/lib/unformat';

  export default {

    name: 'App',

    components: {
      DatePicker
    },

    data() {
      return {
        ptPT: {
          night: 'Noite',
          nights: 'Noites',
          'day-names': ['Dom', 'Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sab'],
          'check-in': 'Chegada',
          'check-out': 'Partida',
          'month-names': ['Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho', 'Julho', 'Agosto', 'Setembro', 'Outubro', 'Novembro', 'Dezembro'],
        },
        dummyStore: null
      };
    },

    methods: {
      //Example of handling user selection

      getUserLocale() {
        //server returns user's currency
        return {
          symbol: '€',
          precision: 2,
          thousand: ".",
          decimal: ","
        }
      },

      getPricing(startDate, nightCount, holdBooking = false) {
        
        /**
         * Example of what a server would return to the frontend
         * 
         * Dynamically get data based on startDate (example: axios)
         *  
         *  prices[]:   [0] - label: string
         *              [1] - cost 1st night: string
         *              [2] - cost 2nd night: string
         *              [3] - cost 3rd night: string
         * 
         *  avgPrice[] :  [0] - label: string
         *                [1] - average price for 1 night stay: string
         *                [2] - average price for 2 night stay: string
         *                [3] - average price for 3 night stay: string
         * 
         * last element in array will be used for pricing for all subsequent nights   
        */

        /**
         * Server returns pricing with label for user's locale (translation server-side)
         * All the calculation show here is for example purposes and should be done on server
         */

        //server returns user's locale
        let currency = this.getUserLocale()

        //server returns pricer per day, based on provided start date.  In this example
        //the last price repeats for the month...

        let serverPricing = this.getPricingMonth()

        let pricesArray = []

        pricesArray[0] = 'Price: '
        //extract the price range based on user selecting and current hover position
        let _index = 0
        let _start = startDate.getDate()
        let _count = _start + (nightCount) - 1

        for(let _p=_start;_p<=_count;_p++) {
          _index++
          pricesArray[_index] = serverPricing[_p]
        }

        //server returns averge pricing
        let avgPriceArray = []
        //server retruns total pricing
        let totalPriceArray = []

        //get the smaller of the night count or pricing array
        let count = Math.min(nightCount, pricesArray.length-1)

        //copy label
        avgPriceArray[0] = 'Averag: '
        totalPriceArray[0] = 'Total: '
        
        //calc average pricing
        for (let i=1;i<=nightCount;i++) {
          if (i==1) {
            avgPriceArray[i] = formatMoney(pricesArray[i], currency)
            totalPriceArray[i] = avgPriceArray[i]
          } else {
            let totalPrice = 0.00
            for(let x=1;x<=i;x++) {
              let amount = unformat(pricesArray[Math.min(x, count)])
              totalPrice = totalPrice + amount 
            }
            let avgPrice = unformat(toFixed(totalPrice / i))
            avgPriceArray[i] = formatMoney(avgPrice, currency)
            totalPriceArray[i] = formatMoney(totalPrice, currency)
          }
        }

        for (let i=1; i<=count; i++) {
          pricesArray[i] = formatMoney(pricesArray[i], currency)
        }
        return {prices: pricesArray, averages: avgPriceArray, totals: totalPriceArray}
      },
      getPricingMonth() {
        //in this example, request 60 days worth of pricing for labels
        //and use a dummy data property to act as the store
        if (!this.dummyStore) {
          this.dummyStore = Array.from({length: 60}, () => Math.floor(Math.random() * 60))
        }

        return this.dummyStore

      },
      fillInPricing(prices, price, num) {
        //TODO: move this to helper...
        let _p = prices.slice(0)
        _p.shift() //remove the label

        for(let v=_p.length;v<num;v++) {
          _p.push(price)
        }
        return _p
      },
      getFinalPrice($event) {

        let userCheckIn = $event.checkIn
        let userCheckOut = $event.checkOut
        let userNumDays = $event.count
        
        //should get pricing again from server 
        let finalPrice = this.getPricing(userCheckIn, userNumDays, true)

        //present data to customer
        console.log(`Checking-in on: ${userCheckIn.toLocaleDateString()}, checking-out on: ${userCheckOut.toLocaleDateString()}`)
        console.log('Number of nights: ', userNumDays)
        console.log('Averge per night: ', finalPrice.averages[finalPrice.averages.length-1])
        console.log('Price per night: ', this.fillInPricing(finalPrice.prices, finalPrice.prices[finalPrice.prices.length-1], userNumDays))
        console.log('Total Price (excl. taxe):', finalPrice.totals[finalPrice.totals.length-1])

        //once user agrees confirm booking...
        //... TODO
      },
      setDynamicTitle(startDate, night) {
        let _p = this.getPricing(startDate, night).prices.slice(0)
        _p.shift() //remove label

        let pricing = unformat(_p,',')

        let highest = Math.max(...pricing)

        let dayPrice = night > pricing.length ? pricing[pricing.length-1] : pricing[night-1]

        if ( dayPrice / highest < 0.8) {
          return `<div><strong style='color: red'>Big Sale !</strong></div>`
        } else { 
          return '' 
        }
      },
      setTextDate(startDate, day, duration) {
        if (!startDate) return null

        let pricing = this.getPricingMonth(startDate, duration)
        let currency = this.getUserLocale()

        let nightPrice = formatMoney(pricing[day.getDate()-1], currency)

        return `<div class="day-label label-bottom">${nightPrice}</div>`
      }
    }
  };

</script>

<style>
  body,
  html {
    font-family: 'Source Sans Pro', sans-serif;
  }

  .box {
    width: 100%;
  }
</style>
