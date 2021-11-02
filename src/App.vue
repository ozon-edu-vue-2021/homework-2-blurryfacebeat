<template>
  <div id="app">
    <TreeView :prop-data="treeData" />
  </div>
</template>

<script>
import treeData from "../public/static/node_modules.json";
import TreeView from "./components/TreeView/TreeView";

export default {
  name: 'App',
  components: {
    TreeView
  },
  data() {
    return {
      treeData
    }
  }
}
</script>

<style lang="css">
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  li {
    list-style: none;
  }

 body {
   font-family: 'Zen Kurenaido', sans-serif;
   font-weight: 700;
 }
</style>