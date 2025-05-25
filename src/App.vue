<script setup>

import SoloGool2 from "@/components/SoloGool-2.vue";
import { ElConfigProvider } from 'element-plus'

</script>

<template>
  <el-config-provider :size="size" :z-index="zIndex">
    <app />
  </el-config-provider>
  <main>
    <SoloGool2>

    </SoloGool2>

  </main>


</template>

<style scoped>
</style>
