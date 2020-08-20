<template>
    <div class="box">
        <!-- <img width="200" height="200" :src="src" alt="Icon"> -->
        <div id="openweathermap-widget-11" :key="componentKey"></div>
        <!-- <h1>{{ cords_id }}</h1> -->
    </div>
</template>

<script>
export default {
    name: 'Skycons',
    props: ["icon", "url_base_icon", "end", "cords_id", "api_key"],
    data() {
        return {
            src: `${this.url_base_icon}${this.icon}${this.end}`,
            componentKey: 0,
        }
    }, 
    created () {   
        window.myWidgetParam ? window.myWidgetParam : window.myWidgetParam = [];
        window.myWidgetParam.push({id: 11,cityid: this.cords_id,appid: this.api_key,units: 'metric',containerid: 'openweathermap-widget-11',  });  
        this.createW();   
    },
    methods: {
        createW() {
            console.log("opop");
            const script = document.createElement('script');
            script.async = true;
            script.charset = "utf-8";
            script.src = "//openweathermap.org/themes/openweathermap/assets/vendor/owm/js/weather-widget-generator.js";
            const s = document.getElementsByTagName('script')[0];
            s.parentNode.insertBefore(script, s);
        }
    },
    watch: {
        cords_id(cords_id, old) {
            console.log(cords_id);
            console.log(old);
            window.myWidgetParam = []; 
            console.log(window.myWidgetParam);  
            window.myWidgetParam.push({id: 11,cityid: cords_id,appid: this.api_key,units: 'metric',containerid: 'openweathermap-widget-11',  });  
            console.log("opopAjdeeee");
            const script = document.createElement('script');
            script.async = true;
            script.charset = "utf-8";
            script.src = "//openweathermap.org/themes/openweathermap/assets/vendor/owm/js/weather-widget-generator.js";
            const s = document.getElementsByTagName('script')[0];
            s.parentNode.insertBefore(script, s);
            this.componentKey += 1;
        }
    }        
}    
</script>

<style scoped>
    .box {
        display: inline-block;
        margin: 0 auto;
        padding-top: 50px;
    }

    @media screen and (max-width: 750px) {
        #openweathermap-widget-11 {
            overflow-x: scroll;
            overflow-y: hidden;
            width: 400px;  
        }
    }
</style>