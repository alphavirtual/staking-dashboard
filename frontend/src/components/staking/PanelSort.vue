<template>
  <tr class="panel-sort-container">
    <th class="mobile">Validator</th>
    <th class="mobile">APR</th>
    <th class="hide-xs">#</th>
    <th class="hide-xs">Status</th>
    <th
      v-for="property in properties"
      :key="property.value"
      :class="{
        'sort-by': sort,
      }"
      class="panel-sort-table-header hide-xs"
    >
      <a
        v-if="sort"
        v-tooltip.top="property.tooltip"
        class="sort-by-link"
        @click="() => orderBy(property.value)"
      >
        {{ property.title }}
        <i class="material-icons">arrow_drop_up</i>
      </a>
      <span v-else>{{ property.title }}</span>
    </th>
  </tr>
</template>

<script>
export default {
  name: `panel-sort`,
  props: {
    sort: {
      type: Object,
      default: null
    },
    properties: {
      type: Array,
      required: true
    },
    showOnMobile: {
      type: String,
      required: true
    }
  },
  methods: {
    hideXs(property) {
      const primaryFields = ["small_moniker", "block_number"]
      const hideFieldMobile = property.value !== this.showOnMobile
      return hideFieldMobile && !primaryFields.includes(property.value)
    },
    orderBy(property) {
      console.log(property)

      const sortBys = this.$el.querySelectorAll(`.sort-by`)
      sortBys.forEach(el => el.classList.remove(`active`, `desc`, `asc`))
      const index = this.properties.findIndex(p => p.value === property)
      const el = sortBys[index]

      if (this.sort.property === property) {
        if (this.sort.order === `asc`) {
          this.sort.order = `desc`
        } else {
          this.sort.order = `asc`
        }
      } else {
        this.sort.property = property
      }
      if (this.sort.order === `asc`) {
        el.classList.add(`asc`)
      } else {
        el.classList.add(`desc`)
      }
      el.classList.add(`active`)
    }
  }
}
</script>

<style>

.mobile {
  display: none
}

.panel-sort-container {
  padding: 1rem;
  border-bottom: 1px solid var(--bc-dim);
}

.panel-sort-table-header {
  font-size: var(--sm);
}

.sort-by i {
  font-size: var(--lg);
  position: relative;
  top: 6px;
  right: 4px;
}

.sort-by a {
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
  color: var(--dim-black);
}

.sort-by a:hover {
  color: var(--link);
}

.sort-by.active a {
  color: var(--txt-black);
}

.sort-by.asc i {
  color: var(--tertiary);
}

.sort-by.desc i {
  transform: rotate(180deg);
  color: var(--tertiary);
}

@media screen and (max-width: 414px) {
    
  .mobile {
    display: table-cell;
  }
  .mobile:nth-child(2) {
    text-align: right;
  }
}

</style>
