<template>
  <ul class="tree-view">
    <TreeItem :propData="propData" />
  </ul>
</template>

<script>
import TreeItem from "@/components/TreeView/TreeItem/TreeItem";

export default {
  name: 'TreeView',
  components: { TreeItem },
  props: {
    propData: {
      type: Object,
      required: true
    }
  },
  created() {
    console.log(this.propData)
  }
}
</script>

<style scoped lang="css">
.tree-view {
  margin-top: 30px;
  margin-left: 30px;
}
</style>