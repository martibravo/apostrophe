<template>
  <div class="apos-modal-tabs">
    <ul class="apos-modal-tabs__tabs">
      <li
        class="apos-modal-tabs__tab" v-for="tab in tabs"
        :key="tab.name"
      >
        <button
          :id="tab.name" class="apos-modal-tabs__btn"
          :aria-selected="tab.name === currentTab ? true : false"
          @click="selectTab"
        >
          {{ $t(tab.label) }}
          <span v-if="tabErrors[tab.name] && tabErrors[tab.name].length" class="apos-modal-tabs__label apos-modal-tabs__label--error">
            {{ tabErrors[tab.name].length }} {{ generateErrorLabel(tabErrors[tab.name].length) }}
          </span>
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'AposModalTabs',
  props: {
    tabs: {
      required: true,
      type: Array
    },
    current: {
      type: String,
      default: ''
    },
    errors: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  emits: [ 'select-tab' ],
  computed: {
    currentTab() {
      return this.current || this.tabs[0].name;
    },
    tabErrors() {
      const errors = {};
      for (const key in this.errors) {
        errors[key] = [];
        for (const errorKey in this.errors[key]) {
          if (this.errors[key][errorKey]) {
            errors[key].push(key);
          }
        }
      }
      return errors;
    }
  },
  methods: {
    generateErrorLabel(errorCount) {
      let label = 'Error';
      if (errorCount > 1) {
        label += 's';
      }
      return label;
    },
    selectTab: function (e) {
      const tab = e.target;
      const id = tab.id;
      this.$emit('select-tab', id);
    }
  }
};
</script>

<style lang="scss" scoped>
.apos-modal-tabs {
  display: flex;
  height: 100%;
}

.apos-modal-tabs__tabs {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin: 0;
  padding: 0;
  background-color: var(--a-base-9);
}

.apos-modal-tabs__tab {
  display: block;
}

.apos-modal-tabs__label {
  display: inline-block;
  padding: 3px;
  border: 1px solid var(--a-base-0);
  font-size: var(--a-type-tiny);
  border-radius: 4px 3px;
  text-transform: uppercase;
  letter-spacing: 1px;
  pointer-events: none;
}

.apos-modal-tabs__label--error {
  border: 1px solid var(--a-danger);
}

.apos-modal-tabs__btn {
  @include apos-button-reset();
  @include type-base;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  height: 60px;
  padding: 25px 10px 25px 20px;
  border-bottom: 1px solid var(--a-base-7);
  color: var(--a-text-primary);
  background-color: var(--a-base-9);
  text-align: left;
  cursor: pointer;
  box-sizing: border-box;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    width: 0;
    background-color: var(--a-primary);
    transition: width 0.25s cubic-bezier(0, 1.61, 1, 1.23);
  }

  &[aria-selected='true'],
  &[aria-selected='true']:hover,
  &[aria-selected='true']:focus {
    background-color: var(--a-background-primary);
    &::before {
      background-color: var(--a-primary);
    }
  }

  &:hover,
  &:focus {
    background-color: var(--a-base-10);
    &::before {
      width: 3px;
      background-color: var(--a-base-5);
    }
  }

  &[aria-selected='true'] {
    &::before {
      width: 6px;
    }
  }
}
</style>
