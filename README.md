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

    first-day the first day of a week (numeric)
                (0: Sunday; 1: Monday by default)


Examples
---------

Set the selected date to 11/20/2016. When a date is selected, go to the page /dates/2016-20-11. Put a separator line before Sundays.

    <datestrip 
        active='2016-11-20'
        link='/dates/'
        :first-day=0
    >

Select the current date. When a date is selected, run the "foo" function. Put a separator line before Mondays (by default).

    <datestrip link='/dates/'>

