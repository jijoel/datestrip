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


Extended Example (as a basic Laravel project)
------------------------------------------
In this example, we are going to show a datestrip on the top of a page. When we click a date, we load another page (the other page could include information specific to the date). We use these Laravel files:

### routes/web.php:

    Route::get('/dates/{date?}', function($date=null){
        return view('dates', compact('date'));
    });

### resources/views/dates.blade.php:

    <!DOCTYPE html>
    <html>
    <head>
        <title>Vue Datestrip</title>
        <link rel="stylesheet" type="text/css" href="/css/app.css">
    </head>
    <body>
    <div id="app">

        <datestrip 
            active='{{ $date }}' 
            link="/dates/"
        >   
        </datestrip>

        <script type="text/javascript" src="/js/app.js"></script>
    </div>
    </body>
    </html>

### package.json:

  "dependencies": {
    "moment": "^2.16.0"
  },


### gulpfile.js:

    const elixir = require('laravel-elixir');

    require('laravel-elixir-vue-2');

    elixir(mix => {
        mix.sass('app.scss')
           .webpack('app.js');
    });

    Elixir.tasks.byName('sass').forEach(function (task) {
        task.watch('resources/assets/components/**/*.scss');
    });
    Elixir.tasks.byName('webpack').forEach(function (task) {
        task.watch('resources/assets/components/**/*.+(vue|js)');
    });

### resources/assets/js/app.js:

    require('./bootstrap');

    Vue.component('Datestrip', require('../components/Datestrip/Datestrip.vue'));

    const app = new Vue({
        el: '#app'
    });

### resources/assets/sass/app.scss:

    // Fonts
    @import url(https://fonts.googleapis.com/css?family=Raleway:300,400,600);

    // Variables
    @import "variables";

    // Bootstrap
    @import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap";

    @import "../components/Datestrip/Datestrip";

Copy this code into each of the files, run `npm install` to install dependencies, `gulp` (or `gulp watch`) to generate app.js and app.css, then open it in the browser.


TODO
-----
* Figure out how to load via npm or another package manager.
