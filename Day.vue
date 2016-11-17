<template>
    <div class="day" @click="clicked" :class="{week: isStartOfWeek}">
        <div class="dow">
            {{ dow }}
        </div>
        <div class="number">
            {{ num }}
        </div>
    </div>
</template>

<script type="text/javascript">

export default {

    props: {
        'date': Object,

        'start': {
            type: Number,
            default: 0,
        }
    },

    computed: {
        dow() {
            return this.date.format('ddd');
        },

        num() {
            return this.date.format('D');
        },

        isStartOfWeek() {
            return this.date.format('d') == this.start
        },
    },

    methods: {
        clicked() {
            this.$emit('changed', this.date)
        }
    }
}
</script>