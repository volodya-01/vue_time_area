<template>
  <div class="outbox">
    <div class="outbox_topoutbox">
      <div class="new_outbox_topoutbox_leftoutbox">
        <div class="outbox_topoutbox_leftoutbox">
          <div class="outbox_topoutbox_leftoutbox_one">压力控制时间段列表</div>
        </div>
        <div class="outbox_topoutbox_rightoutbox">
          <div class="outbox_topoutbox_rightoutbox_one" @click="refresh">重置</div>
          <div class="outbox_topoutbox_rightoutbox_two" @click="addrowdata2">添加</div>
          <!-- <div class="outbox_topoutbox_rightoutbox_three">保存</div> -->
        </div>
      </div>
      <div class="new_outbox_topoutbox_rightoutbox" @click="getlastdata">设置计算条件</div>
    </div>
    <div class="outbox_downoutbox">
      <vue-scroll>
        <div class="child-dom2">
          <el-table
            :data="tableData"
            :header-cell-style="headerStyle"
            :row-style="rowStyle"
            style="width: 100%"
          >
            <el-table-column prop="TimeSta" label="起始时间">
              <template slot-scope="scope">
                <input class="tableinput" placeholder="起始时间" v-model="scope.row.TimeSta">
              </template>
            </el-table-column>
            <el-table-column prop="TimeEnd" label="终止时间">
              <template slot-scope="scope">
                <input class="tableinput" placeholder="终止时间" v-model="scope.row.TimeEnd">
                <!-- @input="inputfun" -->
              </template>
            </el-table-column>
            <el-table-column prop="deletrowtime" label="刪除" width="80px">
              <template slot-scope="scope">
                <span class="tableinput01" @click.prevent="deleteRow(scope.$index, tableData)">
                  <img src="@/assets/delet415.png" alt style="margin-left:10px">
                </span>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </vue-scroll>
    </div>
  </div>
</template>
<script>
import Bus from "@/bus.js";
export default {
  name: "presuretimelist",
  inject: ['reload2'],
  data() {
    return {
  /*     alltimetable: alltime.alltime, */
      /* table */
      headerStyle() {
        return {
          "background-color": "#f0f0f1",
          color: "#6e7b8b",
          "font-size": "12px",
          "font-family": "微软雅黑",
          "text-indent": "5px"
        };
      },
      rowStyle() {
        return {
          height: "34px",
          "line-height": "34px",
          color: "#6e7b8b",
          "text-indent": "5px"
        };
      },
      tableData: [
        {
          CodeID: "C022005000002201",
          TimeEnd: "23:59:00",
          TimeSta: "00:00:00"
        }
      ]
    };
  },

  mounted() {
  /*   console.log("%calltimetable", "color:red", this.alltimetable); */
  },
  methods: {
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },

    /* 向表格中添加一行 */
    addrowdata2() {
      var Coefficient = "";
      var CodeID = "";
      var Lowerlimit = "";
      var TargetData = "";
      var TimeEnd = "";
      var TimeSta = "";
      var Upperlimit = "";
      var newobj = {
        CodeID,
        Coefficient,
        Lowerlimit,
        TargetData,
        TimeEnd,
        TimeSta,
        Upperlimit
      };
      this.tableData.push(newobj);
    },
    refresh(){
      this.reload2()
    },
    /* 输入结束时间触发 */
    /*  inputfun(){
      this.$nextTick(alert("6"))
    } */
    /* 输入后点击触发 */
    getlastdata() {
    /*   console.log("%ctableData", "color:green", this.tableData); */
      let timetable = this.tableData; //将输入后表格的数据赋值给timetable
      let timetableallnumarr = [];
      for (var i = 0; i < timetable.length; i++) {
        /**
         * 时间getlastdata转为分
         * @param time 时间(00:00:00)
         * @returns {string} 时间戳（单位：分）
         */
        var time_to_sec = function(time) {
          var s = "";

          var hour = time.split(":")[0];
          var min = time.split(":")[1];

          s = Number(hour * 60) + Number(min);

          return s;
        };
        var timestanum = time_to_sec(timetable[i].TimeSta); //将开始时间转换为分，并强制转换为Number类型
        var timeendnum = time_to_sec(timetable[i].TimeEnd); //将结束时间转换为分，并强制转换为Number类型
        /* console.log("%ctimestanum", "color:green", timestanum);
        console.log("%ctimeendnum", "color:green", timeendnum); */
        var pretimearr = []; //声明一个变量pretimearr保存timetable中的每一项转化的数组
        for (var j = timestanum; j <= timeendnum; j++) {
          pretimearr.push(j);
        }//二级循环结束
        timetableallnumarr.push(pretimearr)
      }//一级循环结束
/*   console.log("%ctimetableallnumarr", "color:green", timetableallnumarr); */
  //将二维数组转换为一维数组
   var arr1 = timetableallnumarr;
    function flatten(arr) { return [].concat( ...arr.map(x => Array.isArray(x) ? flatten(x) : x) ) }
    var arr2 = flatten(arr1); // 将二维数组转换为一维数组结束
    /* 一维数组去重 */
    arr2 = Array.from(
    new Set(arr2)
    );//数组去重结束
 /*  console.log("%c数组去重后arr2", "color:green", arr2); */
  //创建一个length为1440的全部时间数组，间隔为一分钟
  var defaltalltimetable=[]
  for(let i=0;i<1440;i++){
  defaltalltimetable.push(0)
}

   console.log("%c默认defaltalltimetable", "color:green",defaltalltimetable);
  /* 遍历一维数组按指定项替换全部的时间数组，并返回新的数组 */
    for(var i=0;i<arr2.length;i++){
      /*   var Time=defaltalltimetable[arr2[i]].Time
        var Type=1
        var newobj={Time,Type} */
    // 替换 
    var replace = defaltalltimetable.splice(arr2[i],1,6); //删除1项，插入两项 
    }
    /* 重新整合的数组defaltalltimetable即为满足需求的数组 */
     console.log("%c替换后defaltalltimetable", "color:green",defaltalltimetable);
     /* 将重新整合的数组defaltalltimetable传值给echartsbox */
   Bus.$emit("defaltalltimetablefun",defaltalltimetable)
    }
  }
};
</script>
<style scoped lang="scss">
.outbox {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  align-items: flex-start;
  height: 316px;
  width: 35.5vw;
  margin-left: 50px;
  /*  background-color: #1b4b85; */
  .outbox_topoutbox {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    align-items: flex-start;
    justify-content: space-between;
    height: 28px;
    width: 35.5vw;
    margin: 10px 0;
    .new_outbox_topoutbox_leftoutbox {
      display: flex;
      flex-direction: row;
      flex-wrap: nowrap;
      align-items: flex-start;
      justify-content: flex-start;
    }
    .new_outbox_topoutbox_rightoutbox {
      height: 28px;
      width: 90px;
      font: normal 12px/28px "微软雅黑";
      color: #6e7b8b;
      text-align: center;
      border: 1px #e4e4ec solid;
      background: linear-gradient(to top, #ededef 0%, #ededef 50%, #fff 100%);
      cursor: pointer;
    }
    /*  background-color: #1b4b85; */
    .outbox_topoutbox_leftoutbox {
      display: flex;
      flex-direction: row;
      flex-wrap: nowrap;
      align-items: center;
      justify-content: space-between;
      height: 28px;
      width: 124px;
      margin-left: 20px;
      /*  background-color: #1b4b85; */
      .outbox_topoutbox_leftoutbox_one {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        align-items: center;
        justify-content: flex-start;
        height: 22px;
        width: 120px;
        font: bold 12px/20px "微软雅黑";
        color: #6e7b8b;
        /*  background-color: #1b4b85; */
        & input {
          margin-right: 10px;
        }
        & label {
          font: normal 12px/20px "微软雅黑";
          color: #6e7b8b;
          margin-top: -3px;
        }
      }
    }
    .outbox_topoutbox_rightoutbox {
      display: flex;
      flex-direction: row;
      flex-wrap: nowrap;
      align-items: center;
      justify-content: space-between;
      height: 28px;
      width: 106px;
      margin-right: 20px;
      .outbox_topoutbox_rightoutbox_one,
      .outbox_topoutbox_rightoutbox_two,
      .outbox_topoutbox_rightoutbox_three {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        align-items: center;
        justify-content: space-between;
        height: 18px;
        width: 46px;
        font: normal 12px/18px "微软雅黑";
        color: #6e7b8b;
        text-indent: 20px;
        /*  background-color: #1b4b85; */
        cursor: pointer;
      }
      .outbox_topoutbox_rightoutbox_one {
        background-image: url("../assets/cz.png");
        background-repeat: no-repeat;
        background-position: 0px 3px;
      }

      .outbox_topoutbox_rightoutbox_two {
        background-image: url("../assets/add.png");
        background-repeat: no-repeat;
        background-position: 1px 3px;
      }
      .outbox_topoutbox_rightoutbox_three {
        background-image: url("../assets/save.png");
        background-repeat: no-repeat;
        background-position: 2px 3px;
      }
      /*  background-color: #1b4b85; */
    }
  }

  .outbox_downoutbox {
    height: 193px;
    width: 35.5vw;
    overflow: auto;
    /*  background-color: #1b8529; */
    .child-dom2 {
      /* height: 1993px; */
      width: 33.5vw;
      /*   background-color: #1b3d85; */
      margin: 0 20px;
    }
  }
}
.tableinput01 {
  height: 24px;
  width: 76px;
}
/* tableinput样式 */
.tableinput {
  background-color: #f0f0f1 !important;
  height: 24px;
  width: 180px;
  font: normal 12px "微软雅黑";
  color: #6e7b8b;
  text-indent: 5px;
}
::-webkit-input-placeholder {
  font: normal 8px "微软雅黑";
  color: #bbc1c9;
  text-indent: 5px;
}
::-moz-placeholder {
  font: normal 8px "微软雅黑";
  color: #bbc1c9;
  text-indent: 5px;
} /* firefox 19+ */
:-ms-input-placeholder {
  font: normal 8px "微软雅黑";
  color: #bbc1c9;
  text-indent: 5px;
} /* ie */
input:-moz-placeholder {
  font: normal 8px "微软雅黑";
  color: #bbc1c9;
  text-indent: 5px;
}
</style>


