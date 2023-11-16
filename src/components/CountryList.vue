<!-- Country Catalog Data -->
<template>
<div class="container mt-10 ml-10 mr-10 mt-3">
    <label for="search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
    <div class="">
        <div class="relative ">
            <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
                <svg class="w-4 h-4 text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
                </svg>
            </div>
            <input v-model="searchQuery" id="search" class="block w-full mb-10 p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Search" required>
        </div>
        <div>
            <label>Sort Country Name</label>
            <div class="flex items-center justify-center py-4 md:py-8 flex-wrap">

                <button @click="sortData('asc')" class="text-blue-700 hover:text-white border border-blue-600 bg-white hover:bg-blue-700 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-full text-base font-medium px-5 py-2.5 text-center me-3 mb-3 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-500 dark:bg-gray-900 dark:focus:ring-blue-800">ASC</button>
                <button @click="sortData('desc')" class="text-blue-700 hover:text-white border border-blue-600 bg-white hover:bg-blue-700 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-full text-base font-medium px-5 py-2.5 text-center me-3 mb-3 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-500 dark:bg-gray-900 dark:focus:ring-blue-800">DESC</button>
            </div>
        </div>
    </div>
    <div class="grid grid-cols-2 md:grid-cols-5 gap-4">
        <div v-for="item in paginatedData" :key="item.id">
            <img class="h-auto max-w-full rounded-lg shadow" v-bind:src=item.flags.png>
            <h4 class="mt-2" @click="showDatail(item)">{{item.name.official}}</h4>
            <p>CODES: {{item.cca2}}, {{item.cca3}}</p>
            <p>Native Name: <span v-for="(nativeName, countryName) in item" :key="countryName">
                    {{ nativeName.official }}
                </span></p>
            <p>Alt Name: <span v-for="(alternativeName, altName) in item.altSpellings" :key="altName">
                    {{ alternativeName }},
                </span></p>
            <p>Country Code: {{item.idd.root}}<span v-for="(suffixCode, suffix) in item.idd.suffixes" :key="suffix">
                    {{suffixCode}}
                </span></p>

        </div>
    </div>
    <nav class="isolate inline-flex -space-x-px rounded-md shadow-sm mt-10 mb-10" aria-label="Pagination">
        <button @click="prevPage" :disabled="currentPage === 1" class="relative inline-flex items-center rounded-l-md px-2 py-2 text-gray-400 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0">Prev</button>
        <span class="relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-700 ring-1 ring-inset ring-gray-300 focus:outline-offset-0">Page {{ currentPage }} of {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages" class="relative inline-flex items-center rounded-r-md px-2 py-2 text-gray-400 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0">Next</button>
    </nav>
</div>
<div>
    <!-- Use the modal component with v-if to conditionally render it -->
    <div v-if="showModal">
        <!-- Modal content goes here -->
        <div id="default-modal" tabindex="-1" aria-hidden="true" class="overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full flex">
            <div class="relative p-4 w-full max-w-2xl max-h-full">
                <!-- Modal content -->
                <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                    <!-- Modal header -->
                    <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600">
                        <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                            {{this.dataDetail.name }}
                        </h3>
                        <button @click="hideDatail" type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white" data-modal-hide="default-modal">
                            <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                            </svg>

                        </button>
                    </div>
                    <!-- Modal body -->
                    <div class="p-4 md:p-5 space-y-4">
                        <ul>
                            <li><span>Area Code:</span> {{this.dataDetail.area}}</li>
                            <li><span>Capital:</span> <i v-for="(capitals, capital) in this.dataDetail.capital" :key="capital">
                                    {{ capitals }}
                                </i>
                            </li>
                            <li><span>Regoin:</span> {{this.dataDetail.region}}</li>
                            <li><span>Population:</span> {{this.dataDetail.population}}</li>
                            <li><span>Subregion:</span> {{this.dataDetail.subregion}}</li>
                            <li><span>Timezones:</span> <i v-for="(timezones, timezone) in this.dataDetail.timezones" :key="timezone">
                                    {{ timezones }},
                                </i>
                            </li>
                            <li><span>Languages:</span> <i v-for="(languages, language) in this.dataDetail.languages" :key="language">
                                    {{ languages }}
                                </i>
                            </li>
                        </ul>
                    </div>
                    <!-- Modal footer -->
                    <div class="flex items-center p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600">
                        <button @click="hideDatail" data-modal-hide="default-modal" type="button" class="ms-3 text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600">Close</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Button to close the modal -->
        <button @click="showModal = false">Close Modal</button>
    </div>
</div>
</template>

<script>
import ShowDetail from './ShowDetail.vue';
import axios from "axios";

export default {
    components: {
        ShowDetail,
    },
    data() {
        return {
            data: [],
            currentPage: 1,
            itemsPerPage: 25,
            searchQuery: "",
            sortOrder: 'asc',
            dataDetail: {
                name: '',
                area: '',
                capital: {},
                capitalInfo: {},
                region: '',
                population: 0,
                languages: {},
                subregion: '',
                timezones: {},
                currencies: {},
            },
            showModal: false,

        };
    },
    computed: {
        sortedData() {
            // Clone the original array to avoid mutating it directly
            const itemsCopy = [...this.data];

            // Sort the copied array based on the current sortOrder
            itemsCopy.sort((a, b) => {
                if (this.sortOrder === 'asc') {
                    return a.name.official.localeCompare(b.name.official);
                } else if (this.sortOrder === 'desc') {
                    return b.name.official.localeCompare(a.name.official);
                }
            });

            return itemsCopy;
        },
        paginatedData() {
            const start = (this.currentPage - 1) * this.itemsPerPage;
            const end = start + this.itemsPerPage;
            if (!this.searchQuery) {
                return this.sortedData.slice(start, end);
            }

            return this.sortedData.slice(start, end) ?
                this.sortedData.filter(item => this.fuzzySearch(item.name.official, this.searchQuery)) :
                this.sortedData;

        },
        totalPages() {
            return Math.ceil(this.sortedData.length / this.itemsPerPage);
        },
    },
    watch: {
        currentPage() {
            this.fetchData();
        },
    },
    methods: {
        showDatail(showDatail) {
            // set data to object value
            this.dataDetail.area = showDatail.area;
            this.dataDetail.name = showDatail.name.official;
            this.dataDetail.region = showDatail.region;
            this.dataDetail.population = showDatail.population;
            this.dataDetail.capital = showDatail.capital;
            this.dataDetail.capitalInfo = showDatail.capitalInfo;
            this.dataDetail.languages = showDatail.languages;
            this.dataDetail.subregion = showDatail.subregion;
            this.dataDetail.timezones = showDatail.timezones;
            this.dataDetail.currencies = showDatail.currencies;
            this.showModal = true;

        },
        hideDatail() {
            this.dataDetail.area = '';
            this.dataDetail.name = ''
            this.dataDetail.region = '';
            this.dataDetail.population = 0;
            this.dataDetail.capital = {};
            this.dataDetail.capitalInfo = {};
            this.dataDetail.languages = {};
            this.dataDetail.subregion = {};
            this.dataDetail.timezones = {};
            this.dataDetail.currencies = {};
            this.showModal = false;
        },
        sortData(order) {
            this.sortOrder = order;
        },
        fetchData() {
            // Make a request to country catalog REST API
            axios.get('https://restcountries.com/v3.1/all').then((response) => {
                this.data = response.data;
            });
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        },
        prevPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        },
        fuzzySearch(str, query) {
            // Implement your custom fuzzy search logic here
            // For simplicity, a basic case-insensitive substring match is used
            return str.toLowerCase().includes(query.toLowerCase());
        }
    },
    mounted() {
        if (!this.searchQuery) {
            this.fetchData();
        }

    },
};
</script>

<style scoped>
p,
h3 {
    font-family: 'Poppins', sans-serif;
}

h4 {
    font-size: 20px;
    font-weight: 600;
    cursor: pointer;
}

ul li {
    text-align: left;
}

ul li span {
    font-weight: 600;
}

#default-modal {
    background: rgba(0, 0, 0, 0.9);
}
</style>
