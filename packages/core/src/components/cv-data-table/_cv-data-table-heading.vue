<template>
  <th :aria-sort="sortOrder">
    <button type="button" v-if="sortable" :class="['bx--table-sort', orderClass]" @click="onSortClick">
      <span class="bx--table-header-label">{{ heading }}</span>
      <ArrowDown16 class="bx--table-sort__icon" />
      <Arrows16 class="bx--table-sort__icon-unsorted" />
    </button>
    <span v-else class="bx--table-header-label">{{ heading }}</span>
  </th>
</template>

<script>
import ArrowDown16 from '@carbon/icons-vue/es/arrow--down/16';
import Arrows16 from '@carbon/icons-vue/es/arrows/16';

const nextSortOrder = {
  ascending: 'descending',
  descending: 'none',
  none: 'ascending',
};

export default {
  name: 'CvDataTableHeading',
  components: { ArrowDown16, Arrows16 },
  props: {
    heading: { type: String, required: true },
    sortable: Boolean,
    order: { type: String, default: 'none' },
  },
  computed: {
    sortOrder() {
      if (this.order !== 'ascending' && this.order !== 'descending') {
        return 'none';
      } else {
        return this.order;
      }
    },
    sortText() {
      return this.sortOrder !== 'descending'
        ? 'Sort rows by this header in descending order'
        : 'Sort rows by this header in ascending order';
    },
    orderClass() {
      let result = '';
      if (this.sortOrder === 'descending') {
        result = 'bx--table-sort--active';
      } else if (this.sortOrder === 'ascending') {
        result = 'bx--table-sort--active bx--table-sort--ascending';
      }
      return result;
    },
  },
  model: {
    event: 'sort',
    prop: 'order',
  },
  methods: {
    onSortClick() {
      this.$emit('sort', nextSortOrder[this.sortOrder]);
    },
  },
};
</script>
