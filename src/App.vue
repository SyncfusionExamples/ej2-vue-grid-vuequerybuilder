<template>
  <div class="row" id="app">
    <div class="col-md-6">
    <vue-query-builder :rules="rules" :maxDepth="1" :styled="true" @query-updated="updateQuery"></vue-query-builder>
    </div>
    <div class="col-md-6">
    <ejs-grid ref="grid" :dataSource="data" :query="query">
      <e-columns>
        <e-column field='OrderID' headerText='Order ID' :isPrimaryKey='true' textAlign='Right' width=100></e-column>
        <e-column field='CustomerID' headerText='Customer ID' width=120 textAlign='Left'></e-column>
        <e-column field='Freight' headerText='Freight' textAlign='Center' format='C2' width=80></e-column>
      </e-columns>
    </ejs-grid>
    </div>
  </div>
</template>

<script>
import VueQueryBuilder from 'vue-query-builder';
import Vue from "vue";
import { GridPlugin } from '@syncfusion/ej2-vue-grids';
import { Query, Predicate } from '@syncfusion/ej2-data';

Vue.use(GridPlugin);
export default {
  name: 'app',
  components: { VueQueryBuilder },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      rules: [{
          type: "text",
          id: "CustomerID",
          label: "Customer ID",
          operators: ['startswith', 'equal','notequal','endswith','contains']
        },
        {
          type: "numeric",
          id: "Freight",
          label: "Freight",
        }],
        query: new Query,
        data: [
          { OrderID: 10248, CustomerID: 'VINET', Freight: 32.38 },
          { OrderID: 10249, CustomerID: 'TOMSP', Freight: 11.61 },
          { OrderID: 10250, CustomerID: 'HANAR', Freight: 65.83 },
          { OrderID: 10251, CustomerID: 'VICTE', Freight: 41.34 },
          { OrderID: 10252, CustomerID: 'SUPRD', Freight: 51.3 },
          { OrderID: 10253, CustomerID: 'HANAR', Freight: 58.17 },
          { OrderID: 10254, CustomerID: 'CHOPS', Freight: 22.98 },
          { OrderID: 10255, CustomerID: 'RICSU', Freight: 148.33 },
          { OrderID: 10256, CustomerID: 'WELLI', Freight: 13.97 }
      ]
    }
  },
  methods: {
     updateQuery(value) {
      let query = new Query; let pred;

      //Generate query based on filter builder UI.
      value.children.forEach((item) => {
        if(item.query.value) {
          if(pred === undefined) {
            pred = new Predicate(item.query.rule, item.query.selectedOperator, item.query.value, true);
          } else {
            pred = pred.or(item.query.rule, item.query.selectedOperator, item.query.value, true);
          }
        }
      });

      this.query = pred ? query.where(pred): new Query;
      
      //Refresh the grid, in-order to limit rate of refresh, used setTimeout.
      setTimeout(() => this.$refs.grid.refresh(), 200);
    }
  }
}
</script>

<style>
@import "../node_modules/@syncfusion/ej2-vue-grids/styles/material.css";
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  
  margin-top: 60px;
  
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.children .form-inline {
  padding: 15px;
}
</style>
