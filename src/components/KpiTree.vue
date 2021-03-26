<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <TreeChart :json="tree" @click-node="clickNode" />
  </div>
</template>

<script>
import TreeChart from "./TreeChart";
export default {
  name: "KpiTree",
  props: {
    msg: {
      type: String,
    },
  },
  components: {
    TreeChart,
  },
  data: function() {
    return {
      //pattern: [],
      tree: {
        name: "EDITDA",
        value: { amount: "123", color: "green" },
        delta: { amount: "12%", color: "green" },
        children: [
          {
            name: "Customer Profitability",
            value: { amount: "123", color: "green" },
            delta: { amount: "12%", color: "red" },
            children: [
              {
                name: "MRR",
                value: { amount: "123", color: "orange" },
                delta: { amount: "12%", color: "green" },
                children: [
                  {
                    name: "ACV",
                    value: { amount: "123", color: "orange" },
                    delta: { amount: "12%", color: "red" },
                  },
                ],
              },
              {
                name: "Total CAC",
                value: { amount: "123", color: "green" },
                delta: { amount: "12%", color: "green" },
                children: [
                  {
                    name: "Customer Success",
                    value: { amount: "123", color: "red" },
                    delta: { amount: "12%", color: "green" },
                  },
                  {
                    name: "Sales & Marketing",
                    value: { amount: "123", color: "green" },
                    delta: { amount: "12%", color: "green" },
                  },
                ],
              },
            ],
          },
          {
            name: "NON-CAC related",
            value: { amount: "123", color: "green" },
            delta: { amount: "12%", color: "orange" },
            children: [
              {
                name: "R&D (excl. Support)",
                value: { amount: "123", color: "green" },
                delta: { amount: "12%", color: "green" },
              },
              {
                name: "G&A",
                value: { amount: "123", color: "green" },
                delta: { amount: "12%", color: "green" },
              },
            ],
          },
        ],
      },
    };
  },
  methods: {
    clickNode: function(node) {
      return node;
    },
  },
};
</script>
<style scoped></style>
