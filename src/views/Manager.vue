<template>
  <el-container style="height: 100%; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <Aside/>
    </el-aside>

    <el-container>
      <el-header style="text-align: right; font-size: 12px">
        <Header/>
      </el-header>
      <el-main>
        <router-view/>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
// @ is an alias to /src
import Aside from "@/components/Aside";
import Header from "@/components/Header";

export default {
  name: 'Manager',
  components: {
    Aside,
    Header
  },
  data() {
    return{

    }
  },
  methods: {
  },

}
</script>
