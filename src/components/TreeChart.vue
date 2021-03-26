<template>
  <table v-if="kpiData.name">
    <tr>
      <td
        :colspan="
          Array.isArray(kpiData.children) ? kpiData.children.length * 2 : null
        "
        :class="{
          parent__level:
            Array.isArray(kpiData.children) && kpiData.children.length,
          extend:
            Array.isArray(kpiData.children) &&
            kpiData.children.length &&
            kpiData.extend,
        }"
      >
        <div :class="{ node: true }">
          <div
            :class="Array.isArray(kpiData.class) ? kpiData.class : []"
            @click="$emit('click-node', kpiData)"
          >
            <div class="child">
              <div class="name">
                <span> {{ kpiData.name }}</span>
                <span
                  v-if="
                    Array.isArray(kpiData.children) && kpiData.children.length
                  "
                  @click="toggleExtendChild(kpiData)"
                  class="show__extend"
                ></span>
              </div>
              <div class="child__lower">
                <div v-bind:style="{ color: kpiData.value.color }">
                  <span> {{ kpiData.value.amount }}</span>
                </div>
                <div class="slash">
                  <span>|</span>
                </div>
                <div
                  class="delta"
                  v-bind:style="{ color: kpiData.delta.color }"
                >
                  <i class="fa fa-caret-down" v-if="down(kpiData)"></i>
                  <i class="fa fa-caret-right" v-else-if="right(kpiData)"></i>
                  <i class="fa fa-caret-up" v-else></i>
                  <span v-bind:style="{ color: kpiData.delta.color }">{{
                    kpiData.delta.amount
                  }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </td>
    </tr>
    <tr
      v-if="
        Array.isArray(kpiData.children) &&
          kpiData.children.length &&
          kpiData.extend
      "
    >
      <td
        v-for="(children, index) in kpiData.children"
        :key="index"
        colspan="2"
        class="child__level"
      >
        <TreeChart :json="children" @click-node="$emit('click-node', $event)" />
      </td>
    </tr>
  </table>
</template>

<script>
export default {
  name: "TreeChart",
  props: ["json"],
  data() {
    return {
      kpiData: {},
    };
  },
  watch: {
    json: {
      handler: function(Props) {
        let extendKey = function(jsonData) {
          jsonData.extend =
            jsonData.extend === void 0 ? true : !!jsonData.extend;
          if (Array.isArray(jsonData.children)) {
            jsonData.children.forEach((child) => {
              extendKey(child);
            });
          }
          return jsonData;
        };
        if (Props) {
          this.kpiData = extendKey(Props);
        }
      },
      immediate: true,
    },
  },
  methods: {
    // toggle children
    toggleExtendChild: function(kpiData) {
      kpiData.extend = !kpiData.extend;
      this.$forceUpdate();
    },
    //return caret icon based on child delta color
    down: function(kpiData) {
      if (kpiData.delta.color === "red") return true;
    },
    right: function(kpiData) {
      if (kpiData.delta.color === "orange") return true;
    },
  },
};
</script>

<style scoped>
table {
  border-collapse: separate !important;
  border-spacing: 0 !important;
  width: 80%;
  margin: 3rem auto;
}
table td {
  position: relative;
  vertical-align: top;
  padding: 0 0 3.2rem 0;
  text-align: center;
}
.node {
  margin: -1rem 1rem 0;
}

.child {
  width: 13rem;
  height: 5rem;
  background-color: var(--secondary);
  padding: 1rem;
  margin: auto;
  text-align: center;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: background 300ms linear;
}
.child:hover {
  opacity: 0.85;
}
.child .child__lower {
  position: relative;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.child .name {
  color: var(--text);
}

.child .slash span {
  font-weight: 900;
  color: var(--line-1);
}
.child .fa {
  position: absolute;
  font-size: 1.5rem;
  margin: -0.2rem -1rem;
}
.show__extend {
  position: absolute;
  left: 100%;
  bottom: 30px;
  width: 10px;
  height: 10px;
  padding: 10px;
  top: 0.8rem;
  transform: translate3d(-40px, 0, 0);
  cursor: pointer;
}
.show__extend:before {
  content: "";
  display: block;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 0.15rem solid;
  border-color: var(--line) var(--line) transparent transparent;
  transform: rotateZ(135deg);
  transform-origin: 50% 0 0;
  transition: transform ease 300ms;
}
.show__extend:hover:before {
  opacity: 0.5;
}
.extend .show__extend:before {
  transform: rotateZ(-45deg);
}
.extend::after {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 1rem;
  height: 2rem;
  border-left: 0.15rem solid var(--line);
  transform: translate3d(-3px, 0, 0);
}
.child__level::before {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 100%;
  height: 1rem;
  border-left: 0.15rem solid var(--line);
  transform: translate3d(-1px, 0, 0);
}
.child__level::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  top: -1.1rem;
  border-top: 0.15rem solid var(--line);
}
.child__level:first-child:before,
.child__level:last-child:before {
  display: none;
}
.child__level:first-child:after {
  left: 50%;
  height: 2rem;
  bottom: 100%;
  border: 0.15rem solid var(--line);
  border-radius: 0.25rem 0 0 0;
  border-color: var(--line) transparent transparent var(--line);
  transform: translate3d(2px, 0, 0);
}
.child__level:last-child:after {
  right: 50%;
  height: 2rem;
  border-width: 0.15rem;
  border-style: solid;
  border-color: var(--line) var(--line) transparent transparent;
  border-radius: 0 0.25rem 0 0;
  transform: translate3d(-1px, 0, 0);
}
.child__level:first-child.child__level:last-child::after {
  left: auto;
  border-radius: 0;
  border-color: transparent var(--line) transparent transparent;
  transform: translate3d(-1px, 0, 0);
}

/**/
</style>
