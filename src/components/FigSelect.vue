<template>
  <div class="select-menu">
    <select class="select-menu">
      <slot />
    </select>
    <button
      class="select-menu__button"
      :class="{
        'select-menu__button--active': active,
      }"
      @click="activate"
    >
      <span class="select-menu__label">{{ selected.label }}</span>
      <span class="select-menu__caret"></span>
    </button>
    <ul
      ref="slectList"
      class="select-menu__menu select-menu__menu"
      :class="{
        'select-menu__menu--active': active,
      }"
    >
      <li
        v-for="o in options"
        :key="o.value"
        class="select-menu__item"
        :class="{
          'select-menu__item--selected': o === selected,
        }"
        @click="() => select(o)"
      >
        <span class="select-menu__item-icon"></span
        ><span class="select-menu__item-label">{{ o.label }}</span>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

type SelectValue = number | string;

type SelectOption = {
  value: SelectValue;
  label: string;
  selected: boolean;
};

export default Vue.extend({
  name: "FigSelect",
  data() {
    return {
      value: "" as SelectValue,
      selected: {} as SelectOption,
      options: [] as SelectOption[],
      active: false,
      observer: {} as MutationObserver,
    };
  },
  created() {
    this.init();
  },
  mounted() {
    const observer = new MutationObserver(this.init);
    observer.observe(this.$el, {
      childList: true,
      subtree: true,
      attributes: true,
      characterData: true,
    });
    this.observer = observer;
  },
  beforeDestroy() {
    this.observer.disconnect();
  },
  methods: {
    init() {
      const options =
        this.$slots.default &&
        this.$slots.default
          .filter((o) => o.tag === "option" && o?.data?.attrs?.value)
          .map((o) => ({
            value: o?.data?.attrs?.value as SelectValue,
            label: o?.children && (o?.children[0]?.text as string),
            selected: o.data?.domProps?.selected === true || false,
          }));

      if (options) this.options = options as SelectOption[];

      this.selected =
        (this.selected.value && this.selected) ||
        this.options.find((o) => o.selected) ||
        this.options[0];
    },
    activate() {
      this.active = !this.active;
    },
    select(o: SelectOption) {
      this.selected = o;
      this.value = this.selected.value;
      this.active = false;
      this.$emit("input", this.value);
    },
  },
});
</script>
