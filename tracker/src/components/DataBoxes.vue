<template>
    <div class="grid md:grid-cols-4 gap-4">
        <div class="shadow-md bg-gray-100 p-10 text-center rounded">
            <h3 class="text-3xl text-gray-900 font-bold mb-4">
                Cases
            </h3>

            <div class="text-2xl mb-4">
                <span class="font-bold">New:</span>
                {{numberWithCommas( stats.NewConfirmed)}}
                
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">Total:</span>
                {{numberWithCommas(stats.TotalConfirmed)}}
                
            </div>
            <div class="text-2xl mb-4 text-gray-800">
                <span class="font-bold">Ratio:</span>
                -
                
            </div>
            
        </div>

        <div class="shadow-md bg-blue-100 p-10 text-center rounded">
            <h3 class="text-3xl text-blue-900 font-bold mb-4">
                Active
            </h3>
            <div class="text-2xl mb-4">
                <span class="font-bold">New:</span>
                -
                
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">Total:</span>
                {{numberWithCommas( String(Number(stats.TotalConfirmed) - Number(stats.TotalDeaths) - Number(stats.TotalRecovered)))}}
                
            </div>
            <div class="text-2xl mb-4 text-blue-800">
                <span class="font-bold">Ratio:</span>
                {{findPercentage(Number(stats.TotalConfirmed) - Number(stats.TotalDeaths) - Number(stats.TotalRecovered), stats.TotalConfirmed)}}%
                
            </div>
            
        </div>


        <div class="shadow-md bg-red-200 p-10 text-center rounded">
            <h3 class="text-3xl text-red-900 font-bold mb-4">
                Deaths
            </h3>

            <div class="text-2xl mb-4">
                <span class="font-bold">New:</span>
                {{numberWithCommas(stats.NewDeaths)}}
                
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">Total:</span>
                {{numberWithCommas(stats.TotalDeaths)}}
                
            </div>
            <div class="text-2xl mb-4 text-red-800">
                <span class="font-bold">Ratio:</span>
                {{findPercentage(stats.TotalDeaths, stats.TotalConfirmed)}}%
                
            </div>
        </div>



        <div class="shadow-md bg-green-200 p-10 text-center rounded">
            <h3 class="text-3xl text-green-900 font-bold mb-4">
                Recovered
            </h3>

            <div class="text-2xl mb-4">
                <span class="font-bold">New:</span>
                {{numberWithCommas(stats.NewRecovered)}}
                
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">Total:</span>
                {{numberWithCommas(stats.TotalRecovered)}}
                
            </div>
            <div class="text-2xl mb-4 text-green-800">
                <span class="font-bold">Ratio:</span>
                {{findPercentage(stats.TotalRecovered, stats.TotalConfirmed)}}%
                
            </div>
        </div>
    </div>
</template> 

<script>
export default {
    name: 'DataBoxes',
    props: ['stats'],
    methods: {
        numberWithCommas(num) {
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
        },
        findPercentage(num,div){
            var x = (num/div)* 100
            return x.toFixed(2)
        }
    }

}
</script>