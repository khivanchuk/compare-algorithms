<template>
  <div>
    <div class="field">
      <label class="label">Data Order</label>
      <div class="control">
        <div class="select">
          <select v-model="dataOrder">
            <option disabled>{{ DATA_ORDER_OPTIONS.DEFAULT }}</option>
            <option>{{ DATA_ORDER_OPTIONS.RANDOM }}</option>
            <option>{{ DATA_ORDER_OPTIONS.SORTED }}</option>
            <option>{{ DATA_ORDER_OPTIONS.REVERSED }}</option>
          </select>
        </div>
      </div>
    </div>
    <div class="field is-grouped">
      <p class="control">
        <a class="button is-primary" @click="selectData()">
          Submit
        </a>
      </p>
      <p class="control">
        <a class="button is-light" @click="clearData()">
          Cancel
        </a>
      </p>
    </div>
  </div>
</template>

<script>
import { DATA_ORDER_OPTIONS } from '@/shared/constants';

export default {
  name: 'DataEditor',

  data() {
    return {
      DATA_ORDER_OPTIONS: DATA_ORDER_OPTIONS,
      dataOrder: DATA_ORDER_OPTIONS.DEFAULT,
    };
  },

  methods: {
    selectData() {
      this.$emit('onDataSelected', this.dataOrder);
    },
    clearData() {
      this.dataOrder = DATA_ORDER_OPTIONS.DEFAULT;
    },
  },
};
</script>

<style scoped>
.field:not(:last-child) {
  margin-bottom: 1rem;
}
.field:last-child {
  padding-top: 1rem;
}
</style>
