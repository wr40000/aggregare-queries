<template>
  <div id="side-bar">
    <div class="header"><span>复杂网络社区查询系统</span></div>
    <el-tree
      :data="this.data_tree"
      default-expand-all="true"
      highlight-current="true"
      @node-click="clickData"
    ></el-tree>
    <el-dialog :visible.sync="dialogFormVisible_cx">
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">数据集名称</span>
        <el-input
          v-model="search_useless.jg"
          placeholder="请输入"
          prefix-icon="el-icon-search"
        >
        </el-input>
      </div>
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">数据集文件目录</span>
        <el-input
          v-model="search_useless.hx"
          placeholder="请输入"
          prefix-icon="el-icon-search"
        >
        </el-input>
      </div>
      <!-- <div class="demo-input-suffix">
        <span style="width: 100px;!important"></span>
        <el-input
          v-model="search_useless.zx"
          placeholder="请输入"
          prefix-icon="el-icon-search"
        >
        </el-input>
      </div>
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">误差界限：</span>
        <el-input
          v-model="search_useless.wc"
          placeholder="请输入"
          prefix-icon="el-icon-search"
        >
        </el-input>
      </div>
      <div v-show="!button_focus" class="demo-input-suffix">
        <span style="width: 100px;!important">元路径：</span>
        <el-input
          v-model="search_useless.yl"
          placeholder="请输入"
          prefix-icon="el-icon-search"
        >
        </el-input>
      </div> -->
      <div class="demo-input-suffix-select">
        <span style="width: 100px;!important">图类型</span>
        <el-select v-model="value_yg" placeholder="请选择">
          <el-option
            v-for="item in options_cx"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </div>
      <a class="a" href="javaScript::">数据集文件模板</a>
      <div slot="footer" class="dialog-footer">
        <el-button class="el_button_qx" @click="dialogFormVisible_cx = false">
          取 消
        </el-button>
        <el-button
          class="el_button_qd"
          type="primary"
          @click="dialogFormVisible_cx = false"
        >
          确 定
        </el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import sample from "../data/SampleQueries_choose";
export default {
  name: "SideBar",
  filters: {
    ellipsis(value) {
      if (!value) return "";
      if (value.length > 25) {
        return value.slice(0, 25) + "...";
      }
      return value;
    },
  },
  props: {
    sampleQueries: {
      type: Array,
      default() {
        return [];
      },
    },
    singlePath: {
      type: Array,
      default() {
        return [];
      },
    },
    control_graphShow: {
      type: Object,
      default() {
        return 0;
      },
    },
  },
  data() {
    return {
      windowHeight: 0,
      autoHeight: {
        height: "",
      },
      cardInfoList: "",
      sampleData: sample,
      data_tree: null,
      dialogFormVisible_cx: false,
      search_useless: {
        jg: null,
        hx: null,
        zx: null,
        wc: null,
        yl: null,
      },
      options_cx: [
        {
          value: "选项1",
          label: "同构",
        },
        {
          value: "选项2",
          label: "异构",
        },
      ],
      value_yg: "",
    };
  },
  created() {
    window.addEventListener("resize", this.getHeight);
    this.getHeight();
  },
  destroyed() {
    window.removeEventListener("resize", this.getHeight);
  },
  mounted() {
    this.changeToTree(this.sampleData);
  },
  methods: {
    clickData(row) {
      console.log("************clickData:", row);
      if (row.label === "数据导入") {
        console.log(row.label);
        this.dialogFormVisible_cx = true;
        return;
      }
      // row = row.label
      this.control_graphShow = false;
      this.cardInfoList = row;
      this.cardInfoList.flag = 1;
      this.$emit("choosedQuery", this.cardInfoList, 1);
    },
    getHeight() {
      this.windowHeight = window.innerHeight;
      this.autoHeight.height = this.windowHeight - 40 + "px";
    },
    changeToTree(sampleData) {
      let newArr = new Array(6);
      sampleData.forEach((item, index) => {
        if (index < 6) {
          newArr[index] = {};
          let value = item.query;
          let obj = value;
          if (value === "How many astronaut from Russia") {
            value = "SEA算法";
          } else if (value === "How many films in Danish") {
            value = "LocATC算法";
          } else if (value === "How many politicians in Methodism") {
            value = "VAC算法";
          } else if (value === "How many companies in Munich") {
            value = "ACQ算法";
          } else if (value === "How many films produced by Hal Roach") {
            value = "精确算法";
          } else if (value === "How many books written by Danielle Steel") {
            value = "EVAC算法";
          } else {
            return;
          }
          newArr[index].label = value;
          newArr[index].query = obj;
        }
      });
      let newObj = [
        {
          label: "数据导入",
        },
        {
          label: "数据导出",
        },
        {
          label: "查询图构建",
        },
        {
          label: "元路径查询",
        },
        {
          // .filter(item => item ),
          label: "社区查询算法",
          children: newArr,
        },
      ];
      this.data_tree = newObj;
    },
  },
};
</script>

<style lang="scss" scoped>
// ::v-deep .el-table thead tr th {
//   //表头
//   color: #6096b4 !important;
//   background-color: rgba(189, 205, 214, 1) !important;
//   font-weight: 500 !important;
//   font-size: 22px !important;
//   letter-spacing: 1px;
// }
// ::v-deep .el-table {
//   //空白处
//   background-color: rgba(189, 205, 214, 1) !important;
//   border-radius: 15px !important;
//   overflow: hidden;
// }
// ::v-deep .el-table__row {
//   //奇数行
//   background-color: rgba(189, 205, 214, 1) !important;
//   letter-spacing: 0.5px !important;
//   font-size: 18px !important;
//   font-weight: 300;
//   color: #000000 !important;
// }
// ::v-deep .el-table__row td .cell {
//   //奇数行
//   display: flex !important;
//   align-items: center !important;
//   justify-content: start !important;
//   padding-left: 20px !important;
// }
// ::v-deep .el-table td,
// .building-top .el-table th.is-leaf {
//   //修改行内线
//   border-bottom: 1px solid rgba(189, 205, 214, 1) !important;
// }
// ::v-deep .el-table__body tr.current-row > td {
//   //修改鼠标选中行
//   // background: #EEE9DA !important;
//   background: linear-gradient(90deg, #6096b4, #93bfcf, #bdcdd6) !important;
// }
// ::v-deep .el_table_tooltip {
//   margin: auto;
//   padding-right: 30px;
// }
// // 修改头部背景
// .el_table_container {
//   .el_table_column {
//     .el_table_tooltip {
//       .el_table_tooltip_span {
//       }
//     }
//   }
// }
#side-bar {
  width: 100%;
  height: 93vh;
  background-color: rgba(189, 205, 214, 1);
  .header {
    height: 50px;
    width: 100%;
    color: #6096b4;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: 500;
    font-size: 22px;
    letter-spacing: 1px;
    padding-bottom: 12px;
    padding-top: 12px;
  }
  ::v-deep .el-tree-node__content {
    background-color: rgba(189, 205, 214, 1);
    height: 26px;
    padding-bottom: 12px;
    span {
      font-size: 18px;
    }
  }
  ::v-deep .el-tree--highlight-current
    .el-tree-node.is-current
    > .el-tree-node__content {
    // 设置颜色
    color: #ffffff; // 节点的字体颜色
    font-weight: bold; // 字体加粗
  }
  .demo-input-suffix {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 10px;
    font-size: 16px;
    font-weight: bolder;
  }
  .demo-input-suffix-select {
    display: flex;
    align-items: center;
    justify-content: start;
    margin-bottom: 10px;
    font-size: 16px;
    font-weight: bolder;
  }
  .el_button_qx {
    width: 70px;
    height: 35px;
    background-color: #ebe2e296 !important;
    margin: 0 5px !important;
  }
  .el_button_qd {
    width: 70px;
    height: 35px;
    margin: 0 5px !important;
  }
  a {
    color: #007bff;
    text-decoration: none;
    transition: color 0.3s ease;
    font-size: 18px;
  } /* 鼠标悬停时 */
  a:hover {
    color: #0056b3;
  } /* 已访问链接 */
  a:visited {
    color: #6600cc;
  }
}
</style>
