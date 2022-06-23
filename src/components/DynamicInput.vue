<template>
  <div>
    <div class="w-full mt-4 p-10">
      <b-button variant="outline-primary" @click="addMore()">
        Add ingredient</b-button
      >

      <div v-for="(ing, index) in _props.data" :key="index">
        <div class="flex justify-start ml-2 mt-4">
          <input
            v-model="ing.name"
            placeholder="enter ingredient name"
            class="w-full py-2 border border-indigo-500 rounded"
          />
          <!-- <input
            v-model="ing.unit"
            placeholder="enter ingredient units"
            class="w-full py-2 border border-indigo-500 rounded"
          /> -->

          <input
            v-model="ing.amount"
            placeholder="enter ingredient amount"
            class="w-full py-2 border border-indigo-500 rounded"
          />

          <b-form-select
            v-model="ing.unit"
            :options="unitOptions"
            value="ing.unit"
            size="sm"
            class="mt-3"
          ></b-form-select>

          <b-button variant="danger" @click="remove(index)" v-show="index != 0"
            >Remove</b-button
          >
          <!-- <button
            type="button"
            class="ml-2 rounded-md border px-3 py-2 bg-red-600 text-white"
           
          >
            -
          </button> -->
        </div>
      </div>
    </div>
  </div>
</template>
<!-- class="ml-2 rounded-md border px-3 py-2 bg-red-600 text-white" -->
<script>
export default {
  name: "DynamicInput",
  props: {
    data: Array,
  },
  data() {
    return {
      unitOptions: [
        { value: "", text: "Units", disabled: true },
        { value: "piece", text: "piece" },
        { value: "gr", text: "gr" },
        { value: "kg", text: "kg" },
        { value: "liters", text: "liters" },
        { value: "oz", text: "oz" },
      ],
    };
  },
  methods: {
    addMore() {
      this._props.data.push({
        name: "",
        unit: "",
        amount: "",
      });
    },
    remove(index) {
      this._props.data.splice(index, 1);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
