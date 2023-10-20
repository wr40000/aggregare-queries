<template>
  <div id="side-bar">
    <!-- stripe  条纹样式 -->
    <el-table
      :highlight-current-row="true"
      class="el_table_container"
      :data="sampleData.slice(0, 6)"
      :height="autoHeight.height"
      :header-cell-style="{'font-size':'20px','color':'#303133','font-weight': '520'}"
      @row-click="clickData"
    >
      <el-table-column
        class="el_table_column"
        prop="query"
        label="Query Examples"
        align="center"
      >
        <template slot-scope="scope">
          <el-tooltip class="el_table_tooltip" effect="dark" disabled:false :content="scope.row.query" placement="top" :open-delay="500">
            <span class="el_table_tooltip_span">
              <!-- {{ scope.row.query| ellipsis }} -->
              {{ (scope.row.query == 'How many astronaut from Russia' ? 'Attribute-driven查询算法' : '')| ellipsis }}
              {{ (scope.row.query == 'How many films in Danish' ? 'Attributed community查询算法' : '')| ellipsis }}
              {{ (scope.row.query == 'How many politicians in Methodism' ? 'Vertex-centric查询算法' : '')| ellipsis }}
              {{ (scope.row.query == 'How many companies in Munich' ? 'Sampling-estimation查询算法' : '')| ellipsis }}
              {{ (scope.row.query == 'How many films produced by Hal Roach' ? 'Enumeration-based查询算法' : '')| ellipsis }}
              {{ (scope.row.query == 'How many books written by Danielle Steel' ? 'Size-bounded查询算法' : '')| ellipsis }}
            </span>
          </el-tooltip>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import sample from '../data/SampleQueries_choose';
export default {
  name: "SideBar",
  filters: {
    ellipsis(value) {
      if (!value) return "";
      if (value.length > 25) {
        return value.slice(0, 25) + "...";
      }
      return value;
    }
  },
  props: {
    sampleQueries: {
      type: Array,
      default() {
        return []
      }
    },
    singlePath: {
      type: Array,
      default() {
        return []
      }
    },
    control_graphShow: {
      type: Object,
      default() {
        return 0
      }
    }
  },
  data() {
    return {
      windowHeight: 0,
      autoHeight: {
        height: ''
      },
      cardInfoList:'',
      sampleData:sample
    }
  },
  created() {
    window.addEventListener('resize', this.getHeight)
    this.getHeight()
  },
  destroyed() {
    window.removeEventListener('resize', this.getHeight)
  },
  mounted()
  {

  },
  methods: {
    clickData(row) {
      this.control_graphShow = false
      this.cardInfoList = row
      this.cardInfoList.flag = 1
      this.$emit('choosedQuery',this.cardInfoList, 1);
    },
    getHeight() {
      this.windowHeight = window.innerHeight
      this.autoHeight.height = (this.windowHeight - 40) + 'px';
    }
  },
}
</script>

<style lang="scss" scoped>
  ::v-deep .el-table thead tr th{//表头
    color: #6096B4 !important;
    background-color: rgba(189, 205, 214, 1.0)  !important;
    font-weight: 500 !important;
    font-size: 22px !important;
    letter-spacing: 1px;
  }
  ::v-deep .el-table{//空白处
      background-color: rgba(189, 205, 214, 1.0) !important;
      border-radius: 15px !important;
      overflow: hidden;
  }
  ::v-deep .el-table__row{  //奇数行
      background-color: rgba(189, 205, 214, 1.0)  !important;
      letter-spacing: 0.5px !important;
      font-size: 18px !important;
      font-weight: 300;
      color: #000000 !important;
  }
  ::v-deep .el-table__row td .cell{  //奇数行
    display: flex !important;
    align-items: center !important;
    justify-content: start !important;
    padding-left: 20px !important;
  }
  ::v-deep .el-table td,.building-top .el-table th.is-leaf {//修改行内线
    border-bottom:  1px solid rgba(189, 205, 214, 1.0)  !important;
  }
  ::v-deep .el-table__body tr.current-row>td {//修改鼠标选中行
    // background: #EEE9DA !important;
    background: linear-gradient(90deg,  #6096B4, #93BFCF,#BDCDD6) !important;
  }
// 修改头部背景
.el_table_container{
  .el_table_column{

    .el_table_tooltip{
      .el_table_tooltip_span{

      }
    }

  }
}
</style>
