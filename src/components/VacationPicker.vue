<template>
    <div class="container">
        <h1>{{ header }}</h1>
        <ul class="list-group">
                 <li @click="countryAlert(this.country)" class="list-group-item" 
                v-for="(country, index) in countryData.countries" 
                :key="index">        

                    <span :id="country.id" :title="country.details">
                        {{ index }} - {{country.name}} 
                    </span>
            </li >
        </ul>
        <br>
        <h3> Counter: {{ counter }}</h3>
        <button @click="increment()" class="btn btn-success">+</button>
        <button @click="decrement()" class="btn btn-danger">-</button>
    </div>
</template>

<script>
import countryData from '../data/CountryData'; 
    export default {
        name: 'VacationPicker',
        data() {
            return {
            countryData,
            header: 'Vue Vacation Picker',
            counter:0,  
            alertMessage: " You clicked on ",
            } 
        }, 
        methods: {
            countryAlert() {
                alert(this.alertMessage + this.country)
            },
            increment() {
            this.counter++;
            },
            decrement() {
            this.counter--;
            }
        }  
    }
</script>

<style scoped>

</style>