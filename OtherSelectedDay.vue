<template>
    <div class="month">
        <div class="name" :title="date.format('D MMMM YYYY')">
            {{ date.format('MMM')}}
        </div>
        <div class="selected_day">
            <day :date='date'
                @changed="changed(date)"
            ></day>
        </div>
    </div>
</template>

<script type="text/javascript">
import Day from './Day.vue'

export default {

    components: { Day },

    props: {
        'date': {
            type: Object,
            required: true
        }
    },

    methods: {

        changed(date) {
            this.$emit('changed', date)
        },

    },

}
</script>