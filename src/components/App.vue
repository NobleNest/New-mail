<template>
    <div>
        <select  v-model = "area" v-on:click = "sort()" @change = "filter()">
            <option v-for="item in arrUnique" :value="item.SettlementAreaDescription" v-bind:key="item.Ref">{{ item.SettlementAreaDescription }}</option>
        </select>
        <select v-if = "area != false">
            <option v-for="item in arrFiltered" :value="item.Ref" v-bind:key="item.Ref">{{ item.Description }} -- {{ item.CityDescription }}</option>
        </select>
    </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
    export default {
       data: function() {
           return {
                arr:[],
                arrUnique:[],
                area:false,
                arrFiltered:[],
                arrTemp:[]
           };
        },
        mounted: function(){
             axios.post("https://api.novaposhta.ua/v2.0/json/",{
                    "modelName": "AddressGeneral",
                    "calledMethod": "getWarehouses",
                    "methodProperties": {
                        "CityName": ""
                        },
                    "apiKey": "9a557481f95094531372a9d1b55222c8"
                }).then((response) =>{
                    console.log(response.data.data);
                    this.arr = response.data.data;
                })
        },
        methods: {
            sort: function(){
                var _ = require('lodash');
                this.arrUnique = _.uniqBy(this.arr, 'SettlementAreaDescription'); 
            },
            filter: function(){
                let search = this.area
                this.arrFiltered = _.filter(this.arr, function(o) { 
                    if(o.SettlementAreaDescription == search){
                        if(o.SettlementTypeDescription == "місто"){
                            return o
                        }
                    } 
                });
                console.log(this.arrFiltered);
            }
        }
    }
</script>