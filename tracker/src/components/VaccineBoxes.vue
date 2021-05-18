<template>
    <div class="grid md:grid-cols-3 gap-4">
        <div class="shadow-md bg-gray-100 p-10 text-center rounded">
            <h3 class="text-3xl text-gray-900 font-bold mb-4">
                Total Vaccinations
            </h3>

            <div class="text-2xl mb-4">
                {{numberWithCommas( stats.total_vaccinations)}}
            </div>
            <!-- <div class="text-2xl mb-4">
                <span class="font-bold">Total:</span>
                {{numberWithCommas(stats.TotalConfirmed)}}
                
            </div>
            <div class="text-2xl mb-4 text-gray-800">
                <span class="font-bold">Ratio:</span>
                -
                
            </div>
             -->
        </div>

        <div class="shadow-md bg-blue-100 p-10 text-center rounded">
            <h3 class="text-3xl text-blue-900 font-bold mb-4">
                Partially Vaccinated
            </h3>
            <div class="text-2xl mb-4">
                {{numberWithCommas( stats.people_vaccinated)}}
            </div>
        </div>


        <div class="shadow-md bg-green-200 p-10 text-center rounded">
            <h3 class="text-3xl text-green-900 font-bold mb-4">
                Fully Vaccinated
            </h3>

            <div class="text-2xl mb-4">
                {{numberWithCommas(stats.people_fully_vaccinated)}}
                
            </div>
        </div>
        
    </div>
    <select @change="onChange()" v-model="selected" class="form-select mt-10 block w-full border-4 p-3 rounded">
        <option value="0">
            Select Country
        </option>
        <option v-for="(country, data, index) in countries">
            {{country.location}}
        </option>
    </select>    
</template> 

<script>
export default {
    name: 'VaccineBoxes',
    props: ['stats','countries'],
    emits: ["get-vaccine-country"],
    data() {
        return {
            selected: 0
        }
    },
    methods: {
        numberWithCommas(num) {
            console.log(num)
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
        },
        onChange() {
            console.log(this.selected)
            
            var country = ""
            for (var x in this.countries) {
                if (this.countries[x].location === this.selected){
                    country = this.countries[x]
                }
            }
            console.log(country)
            this.$emit('get-vaccine-country',country)

            // const country = this.countries.find((item) => item.location=== this.selected)
            // console.log(country)
            // this.$emit('get-country', country)
        }
    }

}
</script>