<template>

    <div class="wrap-widget">
        <div class="cc-loader" v-if="loading">
            <div class="spinner"></div>
        </div>

        <div v-else>
            <div v-if="!fetchError">
    	       <highcharts :options="options"></highcharts>
            </div>

            <div v-else class="alert alert-error">
                {{fetchError}}
            </div>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'
    import VueHighcharts from 'vue-highcharts'

    Vue.use(VueHighcharts)

    const api = 'src/js/fakeapi/widget-highcharts-3.json'

	export default {
		name: 'widget-highcharts-3',
        data() {
            return {
                loading:true,
                options: {},
                fetchError: null
            }
        },
        created() {
            this.fetchHighCharts()
        },
        methods: {
            fetchHighCharts() {

                this.fetchError = null

                Vue.axios.get(api, {
                    // params
                }).then(response => {

                    if(response.data.error) {

                        this.showError('data.error : JSON file return an error value')

                    } else {

                        this.loading = false
                        this.options = response.data

                    }

                }).catch(error => {

                    this.showError('Error : JSON file not found OR error during fetching datas')

                })

            },

            // Show error message
            showError(msg) {
                this.fetchError = msg
                this.loading = false
                this.options = {}
            }
        }
	}

</script>
