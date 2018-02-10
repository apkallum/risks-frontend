<template>
<b-container>
<b-row class="my-1" v-for="field in riskFieldsList" :key="field">
    <b-col sm="3"><label>{{ field.FieldTitle }}:</label></b-col>
    <b-col sm="9"><b-form-input :id="`type-${field.Type.toLowerCase()}`" :type="field.Type.toLowerCase()" :value="getValueOfField(field)"></b-form-input></b-col>
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
            getValueOfField: function(Field){
                if (Field.Type === 'Text') {
                    return Field.RiskTypeTextFieldValue.Value;
                }
                if (Field.Type === 'Number') {
                    return Field.RiskTypeNumberFieldValue.Value;
                }
                if (Field.Type === 'Date') {
                    return Field.RiskTypeDateFieldValue.Value;
                }
                if (Field.Type === 'Enum') {
                    return Field.RiskTypeEnumFieldValue.Value;
                }
            }
                    
        }
                
    }
           
       
    

</script>

<style scoped>

</style>