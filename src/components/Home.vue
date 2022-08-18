<template>

    <main v-if="!loading">

        <!-- Data Title -->
        
        <div class="text-center">

            <h2 class="text-3xl font-bold">{{title}}</h2>

            <div class="text-2xl mt-4 mb-10">{{timestamp}}</div>

        </div>

        <!-- Data Boxes -->

        <div class="text-center grid md:grid-cols-2 gap-4">

            <!-- Box 1 -->

            <div class="shadow-md bg-blue-100 text-center rounded">

                <h3 class="text-3xl text-blue-900 font-bold mb-4">Cases</h3>

                <div class="text-2xl mb-4">
                
                    <span class="font-bold">New:</span>

                    {{numberWithCommas(stats.NewConfirmed)}}
                    
                </div>
                
                <div class="text-2xl mb-4">
                
                    <span class="font-bold">Total:</span>

                    {{numberWithCommas(stats.TotalConfirmed)}}
                    
                </div>

            </div>

            <!-- End Box -->
            
            <!-- Box 2 -->

            <div class="shadow-md bg-red-200 text-center rounded">

                <h3 class="text-3xl text-blue-900 font-bold mb-4">Deaths</h3>

                <div class="text-2xl mb-4">
                
                    <span class="font-bold">New:</span>

                    {{numberWithCommas(stats.NewDeaths)}}
                    
                </div>
                
                <div class="text-2xl mb-4">
                
                    <span class="font-bold">Total:</span>

                    {{numberWithCommas(stats.TotalDeaths)}}
                    
                </div>

            </div>

            <!-- End Box -->

        </div>

        <!-- Country Select -->

        <select @change="onChange()" v-model="selected" class="form-select mt-10 block w-full border p-3 rounded">

            <option value="0">Select Country</option>

            <option v-for="country in countries" v-bind:value="country.ID">
            
                {{ country.Country }}
            
            </option>

        </select>
        
    </main>

    <main class="flex flex-col align-center justify-center text-center" v-else>

        <div class="text-gray-500 text-3xl mt-10 mb-6">

            Loading Data

        </div>

    </main>

</template>

<script>

export default {

    name : 'Home',

    components:{},

    data() {

        return{

            selected:0,

            loading:true,

            title:'Global',

            dataDate:'',

            stats:{},

            countries:[]

            // loadingImage: require('../assets/hourglass.gif')

        }

    },

    methods:{

        async fetchCovidData(){

            const res = await fetch('https://api.covid19api.com/summary')

            const data = await res.json()

            return data

        },

        numberWithCommas(x){

            return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");

        },

        onChange(){

            const country = this.countries.find((item) => item.ID === this.selected)

            this.stats = country

            this.title = country.Country

        }

    },

    async created(){

        const data = await this.fetchCovidData()

        this.dataDate = data.Date

        this.stats = data.Global

        this.countries = data.Countries

        this.loading = false

    },

    computed:{

        timestamp : function () {
            
            return moment(this.dataDate).format('MMMM Do YYYY, h:mm:ss a')

        }

    }

}

</script>