<template>
    <div class="month">
        <div class="name" :title="start.format('MMMM YYYY')">
            {{ title }}
        </div>
        <div class="days">
            <day v-for="date in days"
                :date='date' 
                :start=firstDOW
                :class="{ selected_day: isSelected(date) }"
                @changed="changed(date)"
            >
            </day>
        </div>
    </div>
</template>


<script type="text/javascript">
import Day from './Day.vue'

export default {

    components: { Day },

    props: {
        'start': {
            type: Object,
            required: true
        },
        'selected': {
            type: Object,
            required: true
        },
        'count': {
            type: Number,
            required: true
        },
        'firstDOW': {
            type: Number,
            required: true,
        },
    },

    computed: {

        days() {
            var days = [ this.start ]

            for (var d=1; d < this.count; d++) {
                days.push(this.start.clone().add(d, 'd'))
            }
            return days
        },

        title() {
            if (this.count == 1)
                return this.start.format('MMM')
            if (this.count < 4)
                return this.start.format('MMMM')

            return this.start.format('MMMM YYYY')
        },
    },

    methods: {

        isSelected(date) {
            return date.isSame(this.selected, 'day')
        },

        changed(date) {
            this.$emit('changed', date)
        },

    },

}
</script>
