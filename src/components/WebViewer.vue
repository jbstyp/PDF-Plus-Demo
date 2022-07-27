<template>
  <div ref='viewer'></div>
</template>

<script>
/* eslint-disable */
import WebViewer from '@pdftron/pdfjs-express'

const inputFields = [
  {
    "fieldLabel": "First Name",
    "fieldName": "topmostSubform[0].Page1[0].Step1a[0].f1_01[0]",
    "fieldValue": "Alan"
  },
  {
    "fieldLabel": "Last Name",
    "fieldName": "topmostSubform[0].Page1[0].Step1a[0].f1_02[0]",
    "fieldValue": "Wilson"
  },
  {
    "fieldLabel": "Address",
    "fieldName": "topmostSubform[0].Page1[0].Step1a[0].f1_03[0]",
    "fieldValue": "123 Lane Road"
  },
  {
    "fieldLabel": "CityStateZip",
    "fieldName": "topmostSubform[0].Page1[0].Step1a[0].f1_04[0]",
    "fieldValue": "Strongsville, OH 44149"
  },
  {
    "fieldLabel": "FilingStatusSingle",
    "fieldName": "topmostSubform[0].Page1[0].c1_1[0]",
    "fieldValue": "1"
  },
  {
    "fieldLabel": "ChildDependents",
    "fieldName": "topmostSubform[0].Page1[0].Step3_ReadOrder[0].f1_06[0]",
    "fieldValue": 1 * 2000
  },
  {
    "fieldLabel": "OtherDependents",
    "fieldName": "topmostSubform[0].Page1[0].Step3_ReadOrder[0].f1_07[0]",
    "fieldValue": 2 * 500
  },
  {
    "fieldLabel": "ExtraWithholding",
    "fieldName": "topmostSubform[0].Page1[0].f1_12[0]",
    "fieldValue": 500
  }
]

export default {
  name: 'WebViewer',
  props: {
    path: String,
    url: String
  },
  mounted: function () {
    WebViewer({
      path: this.path,
      initialDoc: this.url, // replace with your own PDF file
    }, this.$refs.viewer).then((instance) => {
      // call apis here
      const { documentViewer, annotationManager } = instance.Core;

      documentViewer.addEventListener('documentLoaded', () => {
        documentViewer.getAnnotationsLoadedPromise().then(() => {
          const fieldManager = annotationManager.getFieldManager();
          inputFields.forEach(inputField => {
            const field = fieldManager.getField(inputField.fieldName);
            field.setValue(inputField.fieldValue);
          })

        });
      });
    });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
  width: 100%;
  height: 100vh;
}
</style>
