<template>
<div class="container">
    <!-- {{years}} -->
    <!-- {{ cars }} -->
    <!-- {{ makes }} -->
    <div id="years-container" class="dropdown">
        <label for="years">Choose a Year:</label>
            <select class="option" id="years">
                <option v-for="year in years" :key="year">{{ year }}</option>
            </select>
    </div>

    <div id="make-container" class="dropdown">
        <label for="make">Choose a Make:</label>
            <select class="option" id="make">
                <option v-for="make in makes" :key="make">{{ make }}</option>
            </select>
    </div>

    <div id="model-container" class="dropdown">
        <label for="model">Choose a Make:</label>
            <select class="option" id="make">
                <option v-for="make in makes" :key="make">{{ make }}</option>
            </select>
    </div>
</div>

</template>

<script>
export default {
    name: 'options',

    data() {
        return {
            cars: null
        }
    },

    created() {
        (async () => {
            const response = await fetch(
                'https://parseapi.back4app.com/classes/Carmodels_Car_Model_List?count=1&limit=10000&excludeKeys=Category',
                {
                    headers: {
                    'X-Parse-Application-Id': 'kL1dK6AJq545lNQdpfJoJTI325S7K6cRg3gBxL0N', // This is your app's application id
                    'X-Parse-REST-API-Key': 'ync6hXGnUqdtwsXZDM2yVSmjsIRnRiQJsdtubFeO', // This is your app's REST API key
                    }
                }
        );
        const data = await response.json(); // Here you have the data that you need
        this.cars = data.results
      })();
    },
    computed: {
        years() {
            if (this.cars === null) return {}
            const years = {}
            this.cars.forEach(car => {
                years[car.Year] = true
            })
            return Object.keys(years).map(y => parseInt(y)).sort()
        },

        makes() {
            if (this.cars === null) return {}
            const makes = {}
            this.cars.forEach(car => {
                makes[car.Make] = true
            })
            return Object.keys(makes)
        }
        
    }

}

</script>

<style>

.container {
    padding-left: 15%;
    padding-top:75px;
    display: flex;
    flex-direction: column;

}
.dropdown {
    display: flex;
    flex-direction: column;
    padding-bottom: 75px;
}

.option {
    width: 100px;
}
</style>