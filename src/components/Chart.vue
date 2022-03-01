<template>
    <div>
            <div>
                <ag-grid-vue
                  style="width: 1000px; height: 500px"
                  class="ag-theme-alpine"
                  :columnDefs="columnDefs"
                  @grid-ready="onGridReady"
                  :defaultColDef="defaultColDef"
                  :popupParent="popupParent"
                  :enableRangeSelection="true"
                  :enableCharts="true"
                  :chartThemeOverrides="chartThemeOverrides"
                  :rowData="rowData"
                  :tooltipShowDelay="tooltipShowDelay"
                  :tooltipHideDelay="tooltipHideDelay"
                  @first-data-rendered="onFirstDataRendered">
                </ag-grid-vue>
                <div id="myChart" class="ag-theme-alpine my-chart"></div>
            </div>
        </div>
</template>

<script>
import 'ag-grid-community/dist/styles/ag-grid.css';
import 'ag-grid-community/dist/styles/ag-theme-alpine.css';
import 'ag-grid-enterprise';
import { AgGridVue } from 'ag-grid-vue';
import Tooltip from './Tooltip.vue'

export default {
components: {
    'ag-grid-vue': AgGridVue,
    Tooltip,
},

 data: function () {
    return {
      columnDefs: [
        { field: 'athlete', width: 150, chartDataType: 'category', tooltipField: 'athlete',
        tooltipComponentParams: { color: '#ececec' }, },
        { field: 'age', chartDataType: 'category', sort: 'asc' },
        { field: 'sport', tooltipField: 'sport'},
        { field: 'year', chartDataType: 'excluded' },
        { field: 'gold', chartDataType: 'series' },
        { field: 'silver', chartDataType: 'series' },
        { field: 'bronze' },
      ],
      gridApi: null,
      columnApi: null,
      defaultColDef: {
        editable: true,
        sortable: true,
        flex: 1,
        minWidth: 100,
        filter: true,
        resizable: true,
        tooltipComponent: 'Tooltip'
      },
      tooltipShowDelay: null,
      tooltipHideDelay: null,
      popupParent: null,
      chartThemeOverrides: null,
      rowData: null,
    };
  },
  beforeMount() {
      this.data = this.params.api.getDisplayedRowAtIndex(this.params.rowIndex).data;
      this.color = this.params.color || 'white';
  },
  created() {
    this.popupParent = document.body;
    this.chartThemeOverrides = {
      common: {
        title: {
          enabled: true,
          text: 'Medals by Age',
        },
        legend: {
          position: 'bottom',
        },
      },
      column: {
        axes: {
          category: {
            label: {
              rotation: 0,
            },
          },
        },
      },
    };
  },
  methods: {
    onFirstDataRendered(params) {
      var createRangeChartParams = {
        cellRange: {
          rowStartIndex: 0,
          rowEndIndex: 79,
          columns: ['age', 'gold', 'silver', 'bronze'],
        },
        chartType: 'groupedColumn',
        chartContainer: document.querySelector('#myChart'),
        aggFunc: 'sum',
      };
      params.api.createRangeChart(createRangeChartParams);
    },
    onGridReady(params) {
      this.gridApi = params.api;
      this.gridColumnApi = params.columnApi;

      const updateData = (data) => {
        this.rowData = data;
      };

      fetch('https://www.ag-grid.com/example-assets/wide-spread-of-sports.json')
        .then((resp) => resp.json())
        .then((data) => updateData(data));
    },
  },
};
</script>

<style scoped>

</style>