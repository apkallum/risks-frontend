<template>
<b-container>
<b-row class="my-1" v-for="field in riskFieldsList" :key="field">
    <b-col sm="3"><label>{{ field.FieldTitle }}:</label></b-col>
    <b-col sm="9"><b-form-input :id="`type-${field.Type.toLowerCase()}`" :type="field.Type.toLowerCase()" :value="field.Value"></b-form-input></b-col>
</b-row>
</b-container>
</template>

<script>
    export default {
        props: ['riskFieldsList'],
        data () {
            return {

            }
        },
        methods: {
                    
        }
                
    }
           
       
    

</script>

<style scoped>

</style>