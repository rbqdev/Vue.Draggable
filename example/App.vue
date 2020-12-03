<template>
  <main>
    <a href="https://github.com/SortableJS/Vue.Draggable" target="_blank">
      <img
        style="position: fixed; top: 0; right: 0; border: 0; z-index:99999"
        width="149"
        height="149"
        src="https://github.blog/wp-content/uploads/2008/12/forkme_right_gray_6d6d6d.png?resize=149%2C149"
        class="attachment-full size-full"
        alt="Fork me on GitHub"
        data-recalc-dims="1"
      />
    </a>

    <header class="header">
      <div class="container">
        <img src="./assets/logo.svg" class="header__logo" />

        <div class="header__badges">
          <a
            target="_blank"
            href="https://circleci.com/gh/SortableJS/Vue.Draggable"
            ><img
              src="https://circleci.com/gh/SortableJS/Vue.Draggable.svg?style=shield"
            />
          </a>
          <a target="_blank" href="https://codecov.io/gh/SortableJS/Vue.Draggable"
            ><img
              src="https://codecov.io/gh/SortableJS/Vue.Draggable/branch/master/graph/badge.svg"
            />
          </a>
          <a
            target="_blank"
            href="https://codebeat.co/projects/github-com-sortablejs-vue-draggable-master"
            ><img
              src="https://codebeat.co/badges/7a6c27c8-2d0b-47b9-af55-c2eea966e713"
            />
          </a>
          <a
            target="_blank"
            href="https://github.com/SortableJS/Vue.Draggable/issues?q=is%3Aopen+is%3Aissue"
            ><img
              src="https://img.shields.io/github/issues/SortableJS/Vue.Draggable.svg"
            />
          </a>
          <a target="_blank" href="https://www.npmjs.com/package/vuedraggable"
            ><img src="https://img.shields.io/npm/dt/vuedraggable.svg" />
          </a>
          <a target="_blank" href="https://www.npmjs.com/package/vuedraggable"
            ><img src="https://img.shields.io/npm/dm/vuedraggable.svg" />
          </a>
          <a target="_blank" href="https://www.npmjs.com/package/vuedraggable"
            ><img src="https://img.shields.io/npm/v/vuedraggable.svg" />
          </a>
          <a
            target="_blank"
            href="https://github.com/SortableJS/Vue.Draggable/blob/master/LICENSE"
            ><img
              src="https://img.shields.io/github/license/SortableJS/Vue.Draggable.svg"
            />
          </a>
        </div>
      </div>
    </header>

    <section class="stage container">
      <aside class="stage__sidebar">
        <ul class="sidebar__list" role="tablist">
          <li
            v-for="component in componentList"
            class="sidebar__list-item"
            :class="{'active': currentComponent.name === component.name}"
            :key="component.name"
          >
            <a href="#" @click="setCurrentComponent(component)">
              {{ component.display }}
            </a>
          </li>
        </ul>
      </aside>

      <article class="stage__components">
        <header class="stage__components--header">
          <h2>{{ currentComponent.name }}</h2>
          <a
            class="icon-github"
            target="_blank"
            :href="
              `https://github.com/SortableJS/Vue.Draggable/blob/master/example/components/${currentComponent.name}.vue`
            "
          >
            View code
            <i class="fa fa-github icon-large"></i>
          </a>
        </header>
        
        <div class="stage__components--body">
          <div class="card">
            
            <div class="card-body">
            <component :is="currentComponent.name"></component>

            </div>
          </div>
        </div>
      </article>
    </section>
  </main>
</template>

<script>
import $ from "jquery";

const requireContext = require.context("./components/", false, /\.vue$/);
const components = requireContext.keys().reduce((acc, key) => {
  const component = requireContext(key).default;
  acc[component.name] = component;
  return acc;
}, {});

const showAll = process.env.VUE_APP_SHOW_ALL_EXAMPLES === "true";
if (showAll) {
  const order = Object.keys(components);
  const requireContextDebug = require.context(
    "./debug-components/",
    false,
    /\.vue$/
  );
  requireContextDebug.keys().reduce((acc, key) => {
    const component = requireContextDebug(key).default;
    component.order += order;
    component.display = `DEBUG: ${component.display}`;
    acc[component.name] = component;
    return acc;
  }, components);
}

export default {
  name: "app",
  components,
  data() {
    const componentList = Object.values(components)
      .filter(component => component.show)
      .sort((a, b) => a.order - b.order);
    return {
      componentList,
      currentComponent: componentList[0]
    };
  },
  methods: {
    setCurrentComponent(component) {
      this.currentComponent = component;
    }
  }
};
</script>

<style>
.header {
  width: 100%;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  align-items: center;
  padding: 20px;
}
.header .container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
}

.header__logo {
  max-width: 150px;
  height: 150px;
  margin-bottom: 20px;
}

.header__badges {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.stage {
  display: flex;
  height: calc(100vh - 70px);
  padding: 0 !important;
}

.stage__sidebar {
  padding: 20px;
  width: 220px;
  min-width: 220px;
}
.sidebar__list {
  padding: 0;
  list-style: none;
}

.sidebar__list-item a,
.sidebar__list-item a:hover {
  display: flex;
  width: 100%;
  padding: 5px 12px;
  margin: 4px 0;
  border-radius: 2px;
  color: #34495d;
  text-decoration: none;
}
.sidebar__list-item.active a,
.sidebar__list-item a:hover {
  background: #34495d29;
}

.stage__components {
  border-left: 1px solid #e2e8f0;
  border-right: 1px solid #e2e8f0;
  padding: 20px 30px;
  box-sizing: border-box;
  flex-grow: 1;
  overflow: auto;
  position: relative;
  background: #f9f9f9;
}
.stage__components--header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}
.stage__components--header h2 {
  text-transform: capitalize;
  font-size: 26px;
}
.stage__icon-github {
  position: absolute;
  top: 0;
  right: 0;
  padding: 4px 16px;
  background: #6d757d;
  color: #fff;
  border: 1px solid #e2e8f0;
  border-top: 0;
  border-right: 0;
  border-radius: 0 0 0 4px;
}

</style>
