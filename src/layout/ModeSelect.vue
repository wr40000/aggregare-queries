<template>
  <div style="height: 42px; margin-top: 10px">
    <el-col v-show="false" :span="3">
      <div style="visibility: hidden;">占位</div>
    </el-col>

    <!-- 原代码 -->
    <!-- <el-col :span="4" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
      <el-tooltip content="directly return final result">
        <el-button class="select-button" @click="errorBoundClick()">Error-bound model</el-button>
      </el-tooltip>
    </el-col> -->

    <!-- 原代码 -->
    <!-- <div v-if="errorBoundModelClick === true && model === 'error-bound model'" id="error-bound"> -->
    <div v-if="false" id="error-bound">
      <el-col :span="3" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
        <el-input v-model="errorBound" style="width: 80%;text-align:center;"></el-input>
        <span style="text-align:center; font-size: 12px;">
          error bound
        </span>
      </el-col>

      <el-col :span="4" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
        <el-input v-model="confidenceLevel" style="width: 100%;"></el-input>
        <span style="width: 78px; text-align:center; font-size: 12px;">
          confidence level
        </span>
      </el-col>

      <el-col :span="1" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
        <el-tooltip content="submit">
          <el-button type="primary" class="search-icon" @click="modeSelect('normal')">
            <svg t="1654943281654" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1175" width="200" height="200"><path d="M439.488 960l124.416-169.984-124.416-35.84L439.488 960 439.488 960 439.488 960M0 559.936l353.472 107.072 435.328-369.6-337.408 398.144 377.92 116.736L1024 64.064 0 559.936 0 559.936 0 559.936M0 559.936" p-id="1176" fill="#fff"></path></svg>
          </el-button>
        </el-tooltip>
      </el-col>
    </div>

    <div v-else>
      <el-col v-if="false" :span="8">
        <div style="visibility: hidden;">占位</div>
      </el-col>
    </div>

    <el-col v-if="false" :span="1">
      <div style="visibility: hidden;">占位</div>
    </el-col>

    <el-col :span="2" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
      <el-tooltip content="round by round">
        <el-button :class="['select-first', (button_focus_click == null || button_focus_click == true) ? '' : 'select-button-select', button_focus_click ? (button_focus ? 'select-button-select-active' : '') : '']" @click="change_button_focus(true)">同构</el-button>
        <!-- changeStatus('interactive') -->
      </el-tooltip>
    </el-col>
    <el-col :span="2" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
      <el-tooltip content="round by round">
        <el-button :class="['select-first', (button_focus_click == null || button_focus_click == true) ? '' : 'select-button-select', button_focus_click ? (button_focus ? '' : 'select-button-select-active') : '']" @click="change_button_focus_yg(false)">异构</el-button>
        <!-- changeStatus('interactive') -->
      </el-tooltip>
    </el-col>

    <el-dialog :visible.sync="dialogFormVisible_tg">
      <el-select v-model="value_tg" placeholder="请选择">
        <el-option
          v-for="item in options_tg"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
      <div slot="footer" class="dialog-footer">
        <el-button class="el_button_qx" @click="dialogFormVisible_tg = false">取 消</el-button>
        <el-button class="el_button_qd" type="primary" @click="dialogFormVisible_tg = false">确 定</el-button>
      </div>
    </el-dialog>
    <el-dialog :visible.sync="dialogFormVisible_yg">
      <el-select v-model="value_yg" placeholder="请选择">
        <el-option
          v-for="item in options_yg"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
      <div slot="footer" class="dialog-footer">
        <el-button class="el_button_qx" @click="dialogFormVisible_yg = false">取 消</el-button>
        <el-button class="el_button_qd" type="primary" @click="dialogFormVisible_yg = false">确 定</el-button>
      </div>
    </el-dialog>
    <el-dialog :visible.sync="dialogFormVisible_cx">
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">结构系数：</span>
        <el-input
          placeholder="请输入"
          prefix-icon="el-icon-search"
          v-model="search_useless.jg">
        </el-input>
      </div>
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">核心节点：</span>
        <el-input
          placeholder="请输入"
          prefix-icon="el-icon-search"
          v-model="search_useless.hx">
        </el-input>
      </div>
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">置信度：</span>
        <el-input
          placeholder="请输入"
          prefix-icon="el-icon-search"
          v-model="search_useless.zx">
        </el-input>
      </div>
      <div class="demo-input-suffix">
        <span style="width: 100px;!important">误差界限：</span>
        <el-input
          placeholder="请输入"
          prefix-icon="el-icon-search"
          v-model="search_useless.wc">
        </el-input>
      </div>
      <div class="demo-input-suffix" v-show="!button_focus">
        <span style="width: 100px;!important">元路径：</span>
        <el-input
          placeholder="请输入"
          prefix-icon="el-icon-search"
          v-model="search_useless.yl">
        </el-input>
      </div>
      <div class="demo-input-suffix-select">
        <span style="width: 100px;!important">社区模型：</span>
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

      <div slot="footer" class="dialog-footer">
        <el-button class="el_button_qx" @click="dialogFormVisible_cx = false">取 消</el-button>
        <el-button class="el_button_qd" type="primary" @click="dialogFormVisible_cx = false">确 定</el-button>
      </div>
    </el-dialog>

    <el-col :span="4" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
      <el-tooltip content="round by round">
        <el-button class="select-button_abc" @click="chooseInteractive()">Interactive model</el-button>
        <!-- changeStatus('interactive') -->
      </el-tooltip>
    </el-col>
    <el-col :span="1" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
      <el-tooltip v-if="status === 'interactive'" content="continue">
        <el-button type="primary" class="search-icon" @click="modeSelect('interactive')">
          <svg t="1646027791988" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2864" width="200" height="200" fill="#fff"><path d="M104 0v1024l816-512z" p-id="2865"></path></svg>
        </el-button>
      </el-tooltip>
    </el-col>
    <el-col :span="4" style="display:flex;align-items:center;flex-wrap: nowrap;justify-content: space-around;">
      <el-tooltip content="round by round">
        <el-button class="select-button_abc" @click="inputAttr">查询参数</el-button>
        <!-- changeStatus('interactive') -->
      </el-tooltip>
    </el-col>
  </div>
</template>

<script>
export default {
  name: "ModeSelect",
  props: {
    maxRound: {
      type: Number,
      default() {
        return 0
      }
    },
    round: {
      type: Number,
      default() {
        return 0
      }
    },
    query: {
      type: String,
      default() {
        return ""
      }
    },
    // eslint-disable-next-line vue/prop-name-casing
    control_graphShow: {
      type: Object,
      default() {
        return 0
      }
    }
  },
  data() {
    return {
      button_focus: null,
      button_focus_click: null,
      options_tg: [{
        value: '选项1',
        label: 'facebook'
      }, {
        value: '选项2',
        label: 'Github'
      }, {
        value: '选项3',
        label: 'twitch'
      }, {
        value: '选项4',
        label: 'LiveJournal'
      }, {
        value: '选项5',
        label: 'Twitter-2010'
      }],
      value_tg: '',
      options_yg: [{
        value: '选项1',
        label: 'DBLP'
      }, {
        value: '选项2',
        label: 'IMDB'
      }, {
        value: '选项3',
        label: 'Dbpedia'
      }, {
        value: '选项4',
        label: 'Freebase'
      }, {
        value: '选项5',
        label: 'Yago'
      }],
      options_cx: [{
        value: '选项1',
        label: 'k-core'
      }, {
        value: '选项2',
        label: 'k-truss'
      }],
      search_useless:{
        jg:null,
        hx:null,
        zx:null,
        wc:null,
        yl:null,
      },
      value_yg: '',
      dialogFormVisible_tg:false,
      dialogFormVisible_yg:false,
      dialogFormVisible_cx:false,
      model: "",
      status: "",
      oldRound: undefined,
      oldMaxRound: undefined,
      errorBoundModelClick: false,
      errorBound: "",
      confidenceLevel: ""
    }
  },
  watch: {
    query: {
      handler() {
        this.status = "unselected"
        this.model = "unselected"
        this.errorBound = ""
        this.confidenceLevel = ""
      },
      immediate: true,
      deep: true
    }
    // maxRound: {
    //   handler(newValue, oldValue) {
    //     console.log("maxRound")
    //     console.log(newValue, oldValue)
    //     if (oldValue !== 0) {
    //       this.status = "unselected"
    //       this.model = "unselected"
    //     }
    //     if (this.oldRound === 0 && this.round === 0) {
    //       this.status = "unselected"
    //       this.model = "unselected"
    //     }
    //   },
    //   immediate: true,
    //   deep: true
    // },
    // round: {
    //   handler(newValue, oldValue) {
    //     console.log("round")
    //     console.log(newValue, oldValue)
    //     this.oldRound = oldValue
    //     if (newValue === 0 && this.maxRound !== 0) {
    //       this.status = "unselected"
    //       this.model = "unselected"
    //     }
    //   },
    //   immediate: true,
    //   deep: true
    // }
  },
  methods: {
    errorBoundClick() {
      if (this.maxRound !== 0 && this.model !== "interactive model") {
        this.model = 'error-bound model'
        this.errorBoundModelClick = true;
        this.$message.success("Successfully choose error-bound model!")
      } else {
        this.$message.error("No query was submitted or another model has been chosen!")
      }
    },
    modeSelect(mode) {
      this.control_graphShow.isshow = false
      this.$emit("modeSelect", mode)
      if (this.maxRound !== 0)
        this.status = mode
    },
    chooseInteractive() {
      this.control_graphShow.isshow = true
      if (this.maxRound !== 0 && this.model !== "error-bound model") {
        this.status = "interactive"
        this.$message.success("Successfully choose interactive model!")
        this.model = "interactive model"
      } else {
        this.$message.error("No query was submitted or another model has been chosen!")
        return
      }
      this.$emit("modeSelect", "chooseInteractive")
    },
    change_button_focus(value){
      this.button_focus_click = true
      if(this.button_focus === null){
        this.button_focus = value
      }else{
        this.button_focus = !this.button_focus
      }
      this.dialogFormVisible_tg = true
    },
    change_button_focus_yg(value){
      this.button_focus_click = true
      if(this.button_focus === null){
        this.button_focus = value
      }else{
        this.button_focus = !this.button_focus
      }
      this.dialogFormVisible_yg = true
    },
    inputAttr(){
      this.dialogFormVisible_cx = !this.dialogFormVisible_cx
    }
  }
}
</script>

<style lang="scss" scoped>
.select-button-select-active {
  /* padding: 2px; */
  margin: 0 5px 0 5px;
  height: 42px;
  width: 165px;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 1px;
  color: rgb(255, 255, 255) !important;
  background: rgb(0, 0, 0) !important;
  transition: all 0.5s linear;
}
.select-button-select {
  border: none;
}
.select-first{
  /* padding: 2px; */
  margin: 0 5px 0 5px;
  height: 42px;
  width: 165px;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 1px;
  transition: all 0.5s linear;
  color: #ffffff !important;
  background: rgba(189, 205, 214, 1.0);
}

.select-button-select:hover {
  color: rgb(0, 0, 0) !important;
  background: #6096B4 !important;
}

.select-button-select:focus {
  color: rgb(255, 255, 255) !important;
  background: #7d60b4 !important;
}
.select-button_abc {
  /* padding: 2px; */
  margin: 0 5px 0 5px;
  height: 42px;
  width: 165px;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0.3px;
  color: #000000;
  background: rgba(189, 205, 214, 1.0) !important;
  transition: all 0.5s linear;
}

.select-button_abc:hover {
  color: rgb(255, 255, 255) !important;
  background: #6096B4 !important;
}

.select-button_abc:focus {
  color: rgb(255, 255, 255) !important;
  background: #6096B4 !important;
}
.search-icon{
  color: #000000;
  background: rgba(189, 205, 214, 1.0) !important;
  .icon{
    transform: scale(0.7);
  }
}
.search-icon:hover{
  background: #6096B4 !important;
}
.el_button_qx{
  width: 70px;
  height: 35px;
  background-color: #ebe2e296 !important;
  margin: 0 5px !important;
}
.el_button_qd{
  width: 70px;
  height: 35px;
  margin: 0 5px !important;
}
.demo-input-suffix{
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;
  font-size: 16px;
  font-weight: bolder;
}
.demo-input-suffix-select{
  display: flex;
  align-items: center;
  justify-content: start;
  margin-bottom: 10px;
  font-size: 16px;
  font-weight: bolder;
}
</style>
