<template>
    <div class="container">
        <h1>{{ header }}</h1>
        <div class="row">
            <div class="col-6">
            <ul class="list-group">                    
                        <li class="list-group-item" 
                        @click="countryAlert(country), selectCountry(index)"  
                        v-for="(country, index) in countryData.countries" 
                        :key="country.id">        

                            <span :id="country.id" 
                                :title="country.details">
                                {{ country.id }} - 
                                {{country.name.toUpperCase()}} 
                            </span>
                        </li>  
            </ul>
            <h2> Cheaper Destinations: </h2>
            <select class="form-control-lg"
            v-model="selectedCost"
            @change="filterCountries()">
                <option v-for="(cost, index) in costs"
                    :key="index" 
                    :value="cost">
                        {{ cost }}
                </option>
            </select>
            <ul class="list-group">
                <li v-for="(country, index) in filteredCountries"
                :key="index" class="list-group-item">
                {{ country.name }} (EUR: {{country.cost}})
                </li>
            </ul>
           
            </div></div>
        <div class="col-6">
        <h2> {{ textItem }}</h2>
         <ul class="list-group">  
                <li class="list-group-item" >{{ selectedCountry.id }}</li>
                <li class="list-group-item" >{{ selectedCountry.name }}</li>
                <li class="list-group-item" >{{ selectedCountry.capital }}</li>
                     <li class='list-group-item' v-show="isExpensive"> 
                    <span class="badge bg-danger badge-pill"> Expensive!</span>
                      </li>
                    <li class='list-group-item' v-show="onSale"> 
                    <span class="badge bg-success badge-pill">On sale!</span>
                    </li>
                 
                <li class="list-group-item" >{{ selectedCountry.details }}</li>
                <li class="list-group-item" >
                    <img :src="getImgUrl(selectedCountry.img)" 
                    :alt="selectedCountry.img" 
                    class="img-fluid"> 
                </li>
        <h2> Other Countries </h2>
        <input type="text" v-model="newCountry"
        @keyup.enter="addCountry(newCountry)"
        class="form-control-lg" placeholder="New Country...">
            <button @click="addCountry(newCountry)"
            class="btn btn-info">Add Country</button>
                <ul class="list-group">
                    <li class="list-group-item" v-for="(country, index) in newCountries" :key="index">
                        <br>
                         <h3> {{ country }} </h3>
                    </li>
                </ul>
        </ul>
        </div>            
    </div>
</template>

<script>
//import { computed } from '@vue/runtime-core';
import countryData from '../data/CountryData';
import mixins from '../mixins/mixins';

    export default {
        created() {
            console.log('Component VacationPicker created');
        },
      
        name: 'VacationPicker',
        mixins: [mixins], 
        data() {
            return {
            countryData,
            header: 'Vue Vacation Picker',
            textItem: 'Selected: ',
            counter:0,  
            alertMessage: "You clicked on ",
            selectedCountryIndex: 0,   
            newCountry: '',
            newCountries: [],  
            selectedCost: 1000,
            costs: [1000, 2000, 3000, 4000, 5000, 6000],
            filteredCountries: [],

            } 
        }, 
        methods: {
            countryAlert(country) {
            alert(this.alertMessage + country.name);
            },
            selectCountry(index) {
                this.selectedCountryIndex = index;
            },
            addCountry(country) {
                this.newCountries.push(country);
                this.newCountry = '';
            },
            filterCountries() {
                console.log('Filtered cost:::', this.selectedCost);
                this.filteredCountries = this.countryData.countries
                .filter(country => country.cost < this.selectedCost);
            }
        },  
        computed: {
            selectedCountry() {
                console.log('selectedCountry called');
                return {
                    //kopieer elke eigenschap van het geselecteerde land naar een eigenschap in het geretourneerde(called) object.
                  /*  id: this.countryData.countries[this.selectedCountryIndex].id,
                    name: this.countryData.countries[this.selectedCountryIndex].name,
                    capital: this.countryData.countries[this.selectedCountryIndex].capital,
                    cost: this.countryData.countries[this.selectedCountryIndex].cost,
                    details: this.countryData.countries[this.selectedCountryIndex].details,
                    img: this.countryData.countries[this.selectedCountryIndex].img, */

                //spread operator is efficienter:
                ...this.countryData.countries[this.selectedCountryIndex]
                }
            },
            isExpensive() {
                return countryData.countries[this.selectedCountryIndex].cost > 4000;
            },
            onSale() {
                return countryData.countries[this.selectedCountryIndex].cost < 1000;
            },
        }
    }
</script>

<style scoped>

</style>