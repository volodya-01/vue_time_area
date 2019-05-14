<template>
  <div class="hello" v-if="isRouteAlive">
    <!--  <echartsbox/> -->
    <div class="echartsoutbox">
      <Echarts2/>
    </div>
    <div> <echartsbox/></div>
    <div class="presuretimelistoutbox">
      <presuretimelist/>
    </div>
  </div>
</template>

<script>
import echartsbox from "@/components/echartsbox";
import presuretimelist from "@/components/presuretimelist";
import Echarts2 from "@/components/Echarts2";
export default {
  name: "HelloWorld",
  components: {
    echartsbox,
    presuretimelist,
    Echarts2
  },
  provide() {
    return {
      reload2: this.reload2
    };
  },
  data() {
    return {
      isRouteAlive: true
    };
  },
  methods: {
    reload2() {
      this.isRouteAlive = false;
      this.$nextTick(() => {
        this.isRouteAlive = true;
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello{
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: flex-start;
}
.echartsoutbox{
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: center;
  height: 10px;
  width: 32.5vw;
 margin-left: 86px;
 margin-bottom: 20px;
  background-color: #e8ecec;
}
.presuretimelistoutbox{
width: 50vw
}
</style>
