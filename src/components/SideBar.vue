<template>
  <div>
    <div v-if="isVisible" class="overlay" @click="$emit('close')"></div>
    <div v-if="isVisible" class="sidebar">
      <div class="menu">
        <ul class="list-group">
          <template v-for="(item, index) in menuItems" :key="index">
            <li class="list-group-item" @click="toggleExpand(item)" :class="{ 'active': isSameBranch(activeItem, item) }">
              <span :class="{ 'highlighted': isHighlighted(item) }">{{ item.text }}</span>
              <ul v-if="item.expanded" class="list-group">
                <li v-for="(child, childIndex) in item.children" :key="childIndex" class="list-group-item" @click.stop="toggleExpand(child)">
                  <span :class="{ 'highlighted': isHighlighted(child) }">{{ child.text }}</span>
                  <ul v-if="child.expanded" class="list-group">
                    <li v-for="(subChild, subChildIndex) in child.children" :key="subChildIndex" class="list-group-item" @click.stop="toggleExpand(subChild)">
                      <span :class="{ 'highlighted': isHighlighted(subChild) }">{{ subChild.text }}</span>
                    </li>
                  </ul>
                </li>
              </ul>
            </li>
          </template>
        </ul>
      </div>
    </div>
  </div>
  
</template>

<script>
import menuItems from '@/assets/menuData'
export default {
  name: 'SideBar',
  props: {
    isVisible: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      menuItems: menuItems,
      activeItem: null
    }
  },
  methods: {
    toggleExpand(item) {
      if (this.activeItem === item) {
        item.expanded = !item.expanded;
      } else {
        item.expanded = true;
        this.activeItem = item;
      }
    },
    isSameBranch(item1, item2) {
      if (!item1 || !item2) return false;
      while (item1) {
        if (item1.key === item2.key) return true;
        item1 = this.findParent(item1);
      }
      return false;
    },
    findParent(item, menuItems = this.menuItems) {
      for (const menuItem of menuItems) {
        if (menuItem.children && menuItem.children.some(child => child.key === item.key)) {
          return menuItem;
        } else if (menuItem.children) {
          const parent = this.findParent(item, menuItem.children);
          if (parent) return parent;
        }
      }
      return null;
    },
    isActive(item) {
      return this.activeItem === item;
    },
    isHighlighted(item) {
      return this.isActive(item) || this.isSameBranch(this.activeItem, item);
    }
  }
}
</script>

<style scoped>
.sidebar {
  position: fixed;
  top: 0;
  right: 0;
  height: 100%;
  width: 300px;
  background-color: #000000;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  z-index: 1000;
}

.overlay {
  position: fixed;
  top: 0;
  right: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 900;
}

.menu {
  padding: 20px;
  color: #FFFFFF;
}

.list-group-item {
  cursor: pointer;
}

.active {
  background-color: #DDDDDD;
}

ul {
  list-style-type: none;
}

li {
  text-align: left;
}

.highlighted {
  color: yellow;
}
</style>
