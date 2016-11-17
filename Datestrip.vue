<template>

    <div class="datestrip" :id="id">
        <other-selected-day
            @changed='changed'
            :date='selected' 
            v-show='isSelectedBeforeStart'>
        </other-selected-day>

        <div class="nav left_nav" @click="moveStartDate(-1)">
            <div class="arrow"> ◄ </div> 
        </div>

        <div class="months" v-for="month in months">
            <month 
                @changed='changed'
                :start='month.start' 
                :selected='selected' 
                :count='month.count'
                :firstDOW=firstDay
            >
            </month>
        </div>

        <div class="nav right_nav" @click="moveStartDate(1)">
            <div class="arrow"> ► </div>
        </div>

        <other-selected-day
            @changed='changed'
            :date='selected' 
            v-show='isSelectedAfterEnd'>
        </other-selected-day>

    </div>

</template>

<script type="text/javascript">

import Month from './Month.vue'
import OtherSelectedDay from './OtherSelectedDay.vue'
import moment from 'moment'

export default {

    components: { Month, OtherSelectedDay },

    props: {
        'start': {      // First date to show on screen
            type: String,
        },
        'active': {     // date to show as active
            type: String,
        },
        'link': {       // link to go to when selected
            type: String,
        },
        'function': {   // function to call when selected
        },
        'firstDay': {   // first day of week
            type: Number,
            default: 1,
            validator(value) {
                return value >= 0 && value < 7
            }
        },
    },

    data() {
        return {
            'initial': '',  // first date to show on screen
            'selected': {},
            'width': '',
            'columns': 1,
        };
    },

    computed: {

        id() {
            return 'ds'+Math.round(Math.random()*1000000,0)
        },

        months() {
            var months = [];
            var days = this.numberOfDays;
            var start = this.first.clone();

            while(days > 0) {
                var inMonth = start.daysInMonth() - start.format('D') + 1;
                var count = Math.min(days, inMonth);

                months.push({
                    'start': start.clone(),
                    'count': count,
                });

                days -= count;
                start.add(count, 'd')
            }

            return months
        },

        first() {
            return moment(this.initial, 'YYYY-MM-DD')
        },

        last() {
            return this.first.clone().add(this.numberOfDays, 'd')
        },

        numberOfDays() {
            return this.columns
        },

        isSelectedBeforeStart() {
            return this.selected.isBefore(this.first)
        },

        isSelectedAfterEnd() {
            return this.selected.isSameOrAfter(this.last)
        },
    },

    methods: {

        moveStartDate(direction) {
            this.initial = this.first.clone().add( 
                (direction * this.numberOfDays) 
                + (direction * -1),
                'days'
            ).format('YYYY-MM-DD')
        },

        changed(date) {
            if (this.selected.isSame(date, 'day'))
                this.initial = date.format('YYYY-MM-DD')

            this.selected = date

            if (this.function)
                eval(this.function)(date)

            if (this.link)
                window.location = this.link + date.format('YYYY-MM-DD')
        },

        calculateColumns() {
            var ds = document.getElementById(this.id)

            var full_width = ds.offsetWidth

            // get the known-to-be rendered date object
            var date_width = ds.getElementsByClassName('day')[1].offsetWidth

            // get the count; 
            // subtract 2 for buttons 
            // and 1 for possible other selected day
            this.columns = Math.floor(full_width / date_width) - 3
        },

    },

    created() {
        this.selected = (this.active)
            ? moment(this.active, 'YYYY-MM-DD')
            : moment()

        this.initial = (this.start)
            ? this.start 
            : this.selected.format('YYYY-MM-01')
    },

    beforeDestroy() {
      window.removeEventListener('resize', this.calculateColumns)
    },

    mounted() {
        this.calculateColumns();
        window.addEventListener('resize', this.calculateColumns)
    },

};
</script>

