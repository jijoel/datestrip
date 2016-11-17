Datestrip
==========

A Datestrip object shows a line of dates. Users can select a given date. Functions will run based on the dates, or a page can be reloaded.


Usage
------

    <datestrip></datestrip>

    Vue.component('Datestrip', require('components/Datestrip/Datestrip.vue'));

    const app = new Vue({
        el: '#app'
    });

You can pass these properties:

    start     first date to show on screen
                (first day of this month by default)
    active    date to show as active 
                (today as default)
    link      link to go to when selected
                (/link/YYYY-MM-DD, when selected)
    function  name of function to call when selected


