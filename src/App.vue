<template>
  <div class="page-table scrollable">
    <div class="page-header">
      <h1>Element UI Table</h1>
      <!--<el-breadcrumb separator="/">-->
        <!--<el-breadcrumb-item :to="{ path: '/' }"><i class="mdi mdi-home-outline"></i></el-breadcrumb-item>-->
        <!--<el-breadcrumb-item>Components</el-breadcrumb-item>-->
        <!--<el-breadcrumb-item>Tables</el-breadcrumb-item>-->
        <!--<el-breadcrumb-item>Element UI Table</el-breadcrumb-item>-->
      <!--</el-breadcrumb>-->
    </div>

    <div class="test-header">
      Page Size:
      <select v-on:change="onPageSizeChanged()" id="page-size">
        <option value="10" selected="">10</option>
        <option value="100">100</option>
        <option value="500">500</option>
        <option value="1000">1000</option>
      </select>
    </div>
    <ag-grid-vue style="width: 100%; height: 500px;"
                 class="ag-theme-balham"
                 :gridOptions="gridOptions"
                 @gridReady="onGridReady"
                 :columnDefs="columnDefs"
                 :rowData="rowData"
                 :suppressMovableColumns="true"
                 :suppressCellSelection="true"
                 :suppressColumnMoveAnimation="true"
                 :suppressHorizontalScroll="false"
                 :pagination="true"
                 :paginationPageSize="paginationPageSize"
                 :components="components"
    >
    </ag-grid-vue>
  </div>
</template>

<script>
  import {AgGridVue} from 'ag-grid-vue'
  // import locales from './../SharedComponent/lang/ag-Grid/locales'

  let AgGridAcao = {
    template: '<span>xxx {{ this.params.value }} xxx</span>',
  };

  export default {
    name: 'List',
    data() {
      return {
        gridOptions: null,
        gridApi: null,
        columnApi: null,
        autoGroupColumnDef: null,
        rowSelection: null,
        rowGroupPanelShow: null,
        pivotPanelShow: null,
        paginationPageSize: 10,
        columnDefs: null,
        rowData: null,
        frameworkComponents: null,
        context: null
      }
    },
    components: {
      'ag-grid-vue': AgGridVue,
      agGridAcao: AgGridAcao
    },
    beforeMount() {
      this.gridOptions = {
        defaultColDef: {
          resizable: false,
          sortable: true,
          filter: true,
          filterParams: {
            applyButton: false,
            clearButton: true
          }
        }
      }

      this.context = {
        componentParent: this
      }

      this.components = {
        agGridAcao: AgGridAcao
      };

      this.columnDefs = [
        {headerName: 'Make', field: 'make'},
        {headerName: 'Model', field: 'model'},
        {headerName: 'Price', field: 'price'},
        {
          headerName: 'Action',
          field: 'id',
          cellRendererFramework: 'agGridAcao',
        }
      ]

      this.rowData = [
        {make: 'Toyota', model: 'Celica', price: 35000, id: 1},
        {make: 'Ford', model: 'Mondeo', price: 32000, id: 2},
        {make: 'Porsche', model: 'Boxter', price: 72000, id: 3}
      ]

    },
    mounted() {

    },
    methods: {
      teste(id) {
        console.log('TESTEE CLICK', id)
      },
      onPageSizeChanged(newPageSize){
        let value = document.getElementById('page-size').value
        console.log('selecionado', Number(value))
        this.gridApi.paginationSetPageSize(Number(value))
      },
      onGridReady(agGrid) {
        console.log('grid ready')
        console.log('param', agGrid.api.getDisplayedRowCount())
        agGrid.api.sizeColumnsToFit()
      }
    }
  }
</script>

<style lang="scss" scoped>
  /*@import '../../assets/scss/_variables';*/
  @import "~ag-grid-community/dist/styles/ag-grid.css";
  @import "~ag-grid-community/dist/styles/ag-theme-balham.css";
</style>
