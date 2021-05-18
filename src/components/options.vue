<template>
<div class="container">

    <div class="options-container">

    <!-- YEAR OF THE CAR -->
    <div class="dropdown">
        <label for="years">Choose a Year:</label>
            <select class="option" v-model.number="selectedYear">
                <option v-for="year in years" :key="year">{{ year }}</option>
            </select>
    </div>

    <!-- MAKE OF THE CAR -->
    <div class="dropdown">
        <label for="make">Choose a Make:</label>
            <select class="option" v-model="selectedMake">
                <option v-for="make in makes" :key="make">{{ make }}</option>
            </select>
    </div>

    <!-- MODEL OF THE CAR -->
    <div class="dropdown" v-if="selectedMake">
        <label for="model">Choose a Model:</label>
            <select class="option" v-model="selectedModel">
                <option v-for="model in modelsForSelectedMake" :key="model">{{ model }}</option>
            </select>
    </div>

    <!-- ALL SELECTED -->
    

    </div>

    
    <div class="image-and-name" v-if="imageSearchResults">
        <img :src="imageSearchResults[0].link" alt="car" class="car-image">

        <p>{{ completedCar }}</p>  
    </div>
    


</div>

</template>

<script>
export default {
    name: 'options',

    data() {
        return {
            selectedYear: null,
            selectedMake: null,
            selectedModel: null,
            imageSearchResults: null,
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
        },

        models() {
            if (this.cars === null) return {}
            const models = {}
            this.cars.forEach(car => {
                models[car.Model] = true
            })
            return Object.keys(models)
        },

        modelsForSelectedMake() {
            if (this.selectedMake === null) return null

            const models = []

            this.cars.forEach(car => {
                if (car.Make === this.selectedMake && car.Year === this.selectedYear) {
                    models.push(car.Model)
                }
            })

            console.log(models)
            

            return models
        },

        completedCar() {
            if (this.selectedYear === null || this.selectedMake === null || this.selectedModel === null) return null
            
            
            return `${ this.selectedYear } ${ this.selectedMake} ${ this.selectedModel }`
        }
    },
    watch: {
        completedCar() {
            if (this.completedCar === null) return

            console.log(this.completedCar)
            const uriEncodedCompletedCar = encodeURIComponent('car ' + this.completedCar)
            fetch(`https://www.googleapis.com/customsearch/v1?key=AIzaSyBAbWNb9lZX61eOk0fvJugninCOKGRWPN4&cx=9a82dd91e1a2e8f98&q=${uriEncodedCompletedCar}&searchType=image`)
              .then(response => response.json())
              .then(responseJSON => {
                  this.imageSearchResults = responseJSON.items
              })
        },
        selectedYear() {
            this.imageSearchResults = null
            this.selectedMake = null
            this.selectedModel = null
        }
    }

}

</script>

<style>

.container {
    display: flex;
    align-items: center;
    flex-direction: row;
    justify-content: space-between;
}

.options-container {
    padding-left: 15%;
    padding-right: 75px;
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

.car-image {
    width: 500px;
    height: auto;
}
.image-and-name {
    width: 100%;
    padding-right: 15%;
}


</style>