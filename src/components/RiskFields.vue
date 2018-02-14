<template>
<b-container fluid>
<b-row>
 <b-dropdown id="ddown1" text="Select Risk Type" class="m-sm-2">
<b-dropdown-item v-for="(riskType, index) in allRiskTypes" :key="index" :value="riskType.Title" @click="currentRiskType = riskType, showRiskTitle = true, showAddFieldTypeButton = true, showRiskTypeFields = true, showPreview = true">
{{riskType.Title}}
</b-dropdown-item>
 </b-dropdown>
 <b-dropdown id="ddown1" v-show="showAddFieldTypeButton" text="Add Field to Current Risk Type" class="m-sm-2">
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'text')">Add Text Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'number')">Add Number Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'date')">Add Date Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'enum')">Add Enum Field</b-dropdown-item>
</b-dropdown>
<br>
<b-button variant="primary" size="lg" class="m-sm-2" @click="addNewRiskType(allRiskTypes), showRiskTitle = true, currentRiskType = allRiskTypes.slice(-1)[0],  showPreview = true, showRiskTypeFields = false, showAddFieldTypeButton = false" >Add New Risk Type</b-button>
</b-row>
<br>
<div v-show="showRiskTitle">
<b-row><b-col class="m-sm-2"><strong>Risk Title:</strong></b-col></b-row>
<b-row>
<b-col sm="4"><b-input v-model="currentRiskType.Title" placeholder="Risk Title"></b-input></b-col>
<b-col sm="2"><b-button size="sm" variant="primary" @click="showAddFieldTypeButton = true, showRiskTypeFields = true">Save</b-button></b-col>
</b-row>
</div>
<br>
<div v-show="showRiskTypeFields">
<strong>Risk Type Fields:</strong>
<b-row v-for="(field, index) in currentRiskType.RiskTypeGenericType" :key="index">
    <template v-if="field.TypeAsText === 'enum'">
        <b-col sm="5"><b-input v-model="field.FieldTitle" placeholder="Field Title"></b-input></b-col>
        <b-col sm="5" ><b-input placeholder="Enter comma seperated value" v-model="field.FieldValue" @change="commaSeperatedToEnum(field.FieldValue)"></b-input></b-col>
        <b-col sm="1"><b-button size="sm" @click="deleteFieldFromRisk(currentRiskType, index)">X</b-button></b-col>
        <b-col sm="1"><b-button size="sm">Save</b-button></b-col>
    </template>
    <template v-else>
    <b-col sm="5"><b-input v-model="field.FieldTitle" placeholder="Field Title"></b-input></b-col>
    <b-col sm="5"><b-input :type="field.TypeAsText" v-model="field.FieldValue" placeholder="Field Value"></b-input></b-col>
    <b-col sm="1"><b-button size="sm" @click="deleteFieldFromRisk(currentRiskType, index)">X</b-button></b-col>
    <b-col sm="1"><b-button size="sm">Save</b-button></b-col>
    </template>
</b-row>
<br><br>
</div>
<div v-show="showPreview">
<b-row><b-col class="m-sm-2"><strong>Preview:</strong></b-col></b-row>
<b-row v-for="(field, index) in currentRiskType.RiskTypeGenericType" :key="index">
    <b-col m="4">{{field.FieldTitle}}</b-col>
    <template v-if="field.TypeAsText === 'enum'">
     <b-col m="8"><b-dropdown text="Select Enum Value">
        <b-dropdown-item  v-for="(enumItem, index) in enumResult" :key="index">{{enumItem}}</b-dropdown-item>
        </b-dropdown>
        </b-col>
    </template>
    <b-col m="8" v-else>{{field.FieldValue}}</b-col>
</b-row>
</div>
<br><br><br>
</b-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      allRiskTypes: [],
      currentRiskType: {},
      showRiskTitle: false,
      showAddFieldTypeButton: false,
      showRiskTypeFields: false,
      showPreview: false,
      enumResult: [],
    };
  },
  methods: {
    addNewFieldToRisk: (riskObject, Type) => {
        if (!riskObject.hasOwnProperty('RiskTypeGenericType')){
            riskObject.RiskTypeGenericType = []
        }
        riskObject.RiskTypeGenericType.push({
        TypeAsText: Type,
        FieldTitle: "",
        FieldValue: ""
      });
    },
    addNewRiskType: allRiskTypes => {
      allRiskTypes.push({
        User: "1",
        Title: "",
        RiskTypeGenericType: []
      });
    },
    deleteFieldFromRisk: (riskObject, index) => {
      riskObject.RiskTypeGenericType.splice(index, 1);
    },
    commaSeperatedToEnum: function(value) {
        this.enumResult = value.split(',')
      }

    

  },
  created() {
    // Assuming a logged in user return their RiskTypes & associated RiskTypeFields
    axios
      .get("http://backend.bcoreproject.info/api/risktype/?format=json")
      .then(response => (this.allRiskTypes = response.data));

  },
  
};
</script>

<style scoped>

</style>