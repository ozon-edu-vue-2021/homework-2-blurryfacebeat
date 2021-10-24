<template>
  <li>
    <img class="item-icon" :style="depthMargin" :src="iconItem" alt="icon">
    <component
      class="item-name"
      :is="componentType"
      :href="linkHref"
      target="_blank"
      @click="itemClickHandler"
    >
      {{ propData.name }}
    </component>
    <ul v-if="opened">
      <TreeItem
          v-for="item in propData.contents"
          :key="item.name"
          :prop-data="item"
          :depth="depth + 1"
      />
    </ul>
  </li>
</template>

<script>
import icons from "@/assets/icons";

export default {
  name: 'TreeItem',
  props: {
    propData: {
      type: [Object],
      required: true
    },
    depth: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      opened: false
    }
  },
  methods: {
    itemClickHandler() {
      this.opened = !this.opened;
    }
  },
  computed: {
    depthMargin() {
      return `marginLeft: ${this.depth * 10}px`;
    },

    iconItem() {
      return icons.find((item) => item.type === this.propData.type).file;
    },

    componentType() {
      return this.propData.type === 'link' ? 'a' : 'span';
    },

    linkHref() {
      return this.propData.target;
    }
  }
}
</script>

<style scoped lang="css">
.item-icon {
  width: 16px;
  height: 16px;
  margin-right: 5px;
}

.item-name {
  display: inline-block;
  margin-bottom: 10px;

  font-size: 18px;

  cursor: pointer;
}

.item-name:active {
  color: cadetblue;
}
</style>