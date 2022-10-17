<script setup>
  import phoneData from '../data'
  const data = phoneData
</script>

<template>
  <div>
    <transfer
      :data="data"
      right-title="已选择"
       />
  </div>
</template>

<style scoped>
</style>
