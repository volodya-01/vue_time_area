<template>
  <div class="outbox">
    <div class="outbox_topoutbox">
        <div class="outbox_topoutbox_inbox">
           <!-- 指针线 -->
          <div id="dialogslidingImglineadd"></div>
        <!--   <echarts/> -->
       </div>
    </div>
    <div class="outbox_centeroutbox">
      <div class="outbox_centeroutbox_inbox">
        <div
          v-for="(item,index) in alltimearr"
          :key="index"
          :style="{width:(630/1440)+'px',height: (10)+'px'}"
          :class="item.Type==1?'eventactive':''"
        ></div>
      </div>
    </div>
    <div class="outbox_downoutbox">
        <div class="outbox_downoutbox_inboxnum">
         <div
          v-for="(item,indexnum) in 24"
          :key="indexnum"
          :style="{width:(624/24)+'px',height: (12)+'px'}"
        >{{indexnum}}</div>
      </div>
      <div class="outbox_downoutbox_inbox">
         <div
          v-for="(item,indexline) in 96"
          :key="indexline"
          :style="{width:1+'px'}"
          :class="indexline%4==0?'longkd':'shortkd'"
        ></div>
      </div>
    </div>
  </div>
</template>
<script>
import alltime from '@/const/const'
import Bus from "@/bus.js";
export default {
  name: "echartsbox",
 /*  components:{
    echarts
  }, */
 
  data() {
    return {
      indexline:1,
      type: [],
      alltimetable:alltime.alltime,
      alltimearr:[]
    };
  },
  mounted() {
    /*presuretimelist传过来的值 */
    Bus.$on("defaltalltimetablefun",e=>{
      var _this=this
     _this.alltimearr=e
    })
  /*   console.log("%calltimetable","color:red",this.alltimetable) */
    for (var i = 0; i < 1440; i++) {
      var a;
      if (i % 2 == 0) {
        a = 1;
      } else if(i % 3 == 0) {
        a = 1;
      }else {
        a = 0;
      }
      this.type.push(a)
    };
     let self = this;
    self.LopTime();
    setInterval(function() {
      var mindata = new Date().getHours() * 60 + new Date().getMinutes();
      if (mindata % 15 == 0) {
        self.LopTime();
        console.log(mindata);
      }
    }, 60000); 

  },
  methods:{
   
      LopTime() {
      let timer = new Date();
      let hours = timer.getHours();
      let min = timer.getMinutes();
      let total = hours * 60 + min;
      let localnum = parseInt(total / 15);
      let Insetlinelocationline = 15+ localnum * parseFloat(658 / 96);
      document.getElementById("dialogslidingImglineadd").style.left =
        Insetlinelocationline + "px";
    
    },
  }
};
</script>
<style scoped>
.eventactive {
  background-color: #9dc54e;
}
.outbox {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  height: 193px;
  width: 35.5vw;
  border: 1px solid #6e7b8b;
  margin-left: 200px
  /*     background-color: #6e7b8b */
}
.outbox_topoutbox {
   display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-end;
  height: 134px;
  width: 35.5vw;
  margin-top: 11px
/*   background-color: #1978ec; */
}
.outbox_topoutbox_inbox{
 height: 134px;
  width: 658px;
  margin-right: 20px;
    position: relative;
/*   background-color: #e8ecec; */
}
#dialogslidingImglineadd {
  width: 1px;
  height: 94px;
  background-color: red;
  position: absolute;
  bottom: 5px;
  left: 30px;
  z-index: 99;
}
.outbox_centeroutbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-end;
  height: 10px;
  width: 35.5vw;
  margin: 0 0 9px 0;
  /* background-color: #19ecda */
}
.outbox_centeroutbox_inbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  height: 10px;
  /* width: 628px; */
  /* width: 32.708vw; */
  width: 32.5vw;
  margin-right: 22px;
  background-color: #e8ecec;
}
.outbox_downoutbox {
    display: flex;
  flex-direction:column;
  flex-wrap: nowrap;
  justify-content: flex-end;
  align-items: flex-end;
  height: 23px;
  width: 35.5vw;
   border-bottom: 1px #e4e4ec solid;
    border-top: 1px #e4e4ec solid;
/*   background-image: url("../../../../../../assets/dialogkdc.png");
  background-position-y: 2px */
 /*  background-color: #ec19c9; */
}
.outbox_downoutbox_inboxnum{
   display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  height: 12px;
  /* width: 631px; */
  /* width: 32.865vw; */
  width: 32.657vw;
  font: normal 10px/4px "微软雅黑";
  color:#6e7b8b;
  margin-right: 22px;
}
.outbox_downoutbox_inbox{
    display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: center;
  height: 4px;
  /* width: 628px; */
   /* width: 32.708vw; */
   width: 32.5vw;
  margin-right: 22px;
 /*  background-color: #e8ecec; */
}
.longkd{
 height: 4px;
  background-color: #bbbbc4;
}
.shortkd{
 height: 2px;
  background-color: #bbbbc4;
}
</style>


