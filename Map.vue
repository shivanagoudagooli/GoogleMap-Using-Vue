<template >
	<div class="back">
        <div class="row">
            <div class="col-md-12">    
                <!-- <div>
                     {{info}}
                </div>
             -->
                <br/>
                <div class="col-md-10">
                    <gmap-map :center="center" :zoom="7" class="map-data">     
                        <gmap-info-window :key="i+'info'" v-for="(m,i) in markers" :options="infoOptions" :position="m.position" :opened="infoWinOpen" @closeclick="infoWinOpen=false">
                            <p><b>Product/Truck type : </b> {{infoContent.type}}</p>
                            <p><b>Height of truck : </b> {{infoContent.height}}</p>
                            <p><b>Approximate weight : </b> {{infoContent.weight}}</p>
                            <p><b>Customer name : </b> {{infoContent.name}}</p>
                            <p><b>Shipping street : </b> {{infoContent.street}}</p>
                        </gmap-info-window>

                        <gmap-marker :key="i+'marker'" v-for="(m,i) in markers" :position="m.position" :clickable="true" :draggable="true" @click="toggleInfoWindow(m,i)"
                        :icon="m.icon">

                        </gmap-marker>
                    </gmap-map>
                </div>
                <div class="col-md-2 form-group" id="date">
                    <label for="email" class="date">Select date:</label>
                    <date-picker v-model="date" format="DD-MM-YYYY" lang="en"></date-picker>
                </div>
            </div>
        </div>
	</div>
</template> 

<script>
    
    import DatePicker from 'vue2-datepicker'
    import {gmapApi} from 'vue2-google-maps'	
	import Vue from 'vue';
    import axios from 'axios';
	import * as VueGoogleMaps from 'vue2-google-maps'
		
	Vue.use(VueGoogleMaps, {
		load: {
			key: 'AIzaSyBu1BLbaOJ-C6m-Gzgwa89fk82Q0lUmkmM',
            libraries: 'places',
        },
	});

    export default{
 
        data(){

            return {

                date  : new Date(),

                info:null,

                center: { lat: 47.376332, lng: 8.547511 },
                
                infoContent: {},
                
                infoWindowPos: null,
                
                infoWinOpen: false,
                
                currentMidx: null,
                
                //optional: offset infowindow so it visually sits nicely on top of our marker
                
                infoOptions: { pixelOffset: { width: 0, height: -35 } },
                
                markers: [
                {
                    
                    position: { lat: 45.376332, lng: 8.547511 },

                    infoText: 'Marker 1',
                    
                    icon : 'http://127.0.0.1:5501/assets/images/markers/marker-green.png'
                }, 
                {
                    position: { lat: 40.374592, lng: 65.548867 },

                    infoText: 'Marker 2',
                    
                    icon : 'http://127.0.0.1:5501/assets/images/markers/marker-red.png'
                }, 
                {
                    position: { lat: 50.379592, lng: 45.547511 },

                    infoText: 'Marker 3',
                    
                    icon : 'http://127.0.0.1:5501/assets/images/markers/marker-white.png'
                },
                {
                    position: { lat: 25.379592, lng: 25.549867 },

                    infoText: 'Marker 4',
                    
                    icon : 'http://127.0.0.1:5501/assets/images/markers/marker-blue.png'
                },
                {
                    position: { lat: 12.379592, lng: 80.049867 },

                    infoText: 'Marker 5',
                    
                    icon : 'http://127.0.0.1:5501/assets/images/markers/marker-yellow.png'
                }
                ]
            }
        },

        computed : {
					
			google: gmapApi
        },

        beforeMount(){

            this.getData();

            for(var i in this.markers){
                this.toggleInfoWindow(this.markers[i],i)
            }
        },

        methods : {

            getData(){

                const data = {};

                data['startParameters'] = [];

                axios.defaults.headers.common["Authorization"] = 'bearer d9e21e41691e29eb67f0bff1d4648e56b336be5f';

                axios.post('http://40.74.14.223/Motrac/odata/data/searches(d9627f14-d69d-4be9-a1bb-3e0bb137c5e1)/performSearch',data)
                .then(resp=>{this.info = resp
                }).catch(error=>{

                })

                // function (resp) {
                //                     //mylog("load resp", resp);
                //                     if(resp != undefined && resp.body != undefined && resp.body.value != undefined) {
                                       
                //                         for(let i = 0; i < resp.body.value.length; i++) {
                //                             let obj = Main.parseREST(resp.body.value[i].parameters);
                                           
                //                             obj.sortedLorryCode = obj.Lorry.Code;
                                            
                //                             t.orders.push(obj);
                //                         }
                //                     }
            },

             toggleInfoWindow: function(marker, idx) {
                this.infoWindowPos = marker.position;
                this.infoContent = {
                    type : 'type',
                    height : '200',    
                    weight : '200',    
                    name : marker.infoText,
                    street  : 'street' + idx    
                };
                //check if its the same marker that was selected if yes toggle
                if (this.currentMidx == idx) {
                this.infoWinOpen = !this.infoWinOpen;
                }
                //if different marker set infowindow to open and reset current marker index
                else {
                this.infoWinOpen = true;
                this.currentMidx = idx;
                }
            }
        },

        components : {
            DatePicker
        }
    };
</script>

<style>
    .gm-ui-hover-effect{

        display: none !important;
    }
    #date{
        text-align: right !important;
    }
    .back{
        background: #a8222b;
          margin-left: 10px;
            margin-right: 10px;
            height:100%;

    }
    .date{
        color:white;
    }
    .map-data{
   border-style: solid;
   border-width: thick;
   width: 100%;
   height: 800px;
   border-color: white;
    }
</style>