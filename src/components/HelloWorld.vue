<template>
  <div class="hello" v-if="isRouteAlive">
  <echartsbox/>
  <div>
    <presuretimelist/>
  </div>
  </div>
</template>

<script>
import echartsbox from "@/components/echartsbox";
import presuretimelist from "@/components/presuretimelist"
export default {
  name: "HelloWorld",
  components: {
   echartsbox,
   presuretimelist
  },
   provide () {
    return {
      reload2: this.reload2
    }
  },
  data() {
    return {
      isRouteAlive: true,
    };
  },
  methods:{
     reload2 () {
      this.isRouteAlive = false
      this.$nextTick (() => {
        this.isRouteAlive = true
      })
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
