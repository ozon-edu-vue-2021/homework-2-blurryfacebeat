<template>
  <li>
    <div :class="['item-container']" @click="itemClickHandler(propData.name)" ref="tree-item">
      <img class="item-icon" :style="depthMargin" :src="iconItem" alt="icon">
      <component
          class="item-name"
          :is="componentType"
          :href="linkHref"
          target="_blank"
          :style="`color: ${opened ? '#3c797b' : ''}`"
      >
        {{ propData.name }}
      </component>
    </div>
    <ul v-if="opened">
      <TreeItem
          v-for="(item) in propData.contents"
          :key="item.name"
          :prop-data="item"
          :depth="depth + 1"
      />
    </ul>
  </li>
</template>

<script>
import { eventBus } from "@/main";
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
    },
    idx: {
      type: Number
    }
  },
  data() {
    return {
      opened: false,
      activeItem: ''
    }
  },
  mounted() {
    this.setFolderOpenByLink();
    eventBus.$on('removeActiveClass', () => {
      this.$refs["tree-item"]?.classList.remove('active');
    });
  },
  methods: {
    itemClickHandler(query) {
      this.setItemActive();
      this.setFolderOpened();
      this.setURLQuery(query);
    },

    setFolderOpened() {
      if (this.propData.type === 'directory') this.opened = !this.opened;
    },

    setItemActive() {
      if (this.propData.type !== 'directory') {
        eventBus.$emit('removeActiveClass');
        this.$refs["tree-item"]?.classList.add('active');
      }
    },

    setURLQuery(query) {
      if (this.propData.type === 'directory') {
        const oldUrl = `http://${window.location.href.split('/')[2]}`;
        const newUrl = new URL(oldUrl);

        newUrl.searchParams.append('folder', query);
        window.history.pushState('', 'folder', newUrl.href);
      }
    },

    setFolderOpenByLink() {
      if (this.urlQueryParams === this.propData.name) this.opened = true;
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
    },

    urlQueryParams() {
      return location.search.split('=')[1];
    },

    activeItemClass() {
      return this.propData.name === this.activeItem ? 'active' : '';
    }
  }
}
</script>

<style scoped lang="css">
.item-container {
  max-width: max-content;
  padding: 0 15px;
  user-select: none;

  cursor: pointer;
  outline: none;
}

.item-container:hover .item-name {
  color: cadetblue;
}

.item-container:active .item-name {
  color: #3c797b;
}

.item-container.active {
  background-color: rgba(55, 110, 112, 0.3);
  border-radius: 10px;
}

.item-icon {
  width: 16px;
  height: 16px;
  margin-right: 5px;
}

.item-name {
  display: inline-block;
  margin-bottom: 10px;

  font-size: 18px;

  transition: all 0.1s ease;
}
</style>