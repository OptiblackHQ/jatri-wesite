<template>
  <div>
    <div
      v-bind:class="errorOccured && 'border-[#E0293B]'"
      class="
        flex
        justify-between
        px-2
        py-[10px]
        border border-[#DBDBDB]
        rounded
        mt-[10px]
        w-full
        h-[48px]
      "
    >
      <input
        class="
          font-inter
          h-full
          w-full
          outline-none
          overflow-x-hidden
          text-sm
          font-normal
          text-blackPrimary text-left
          placeholder-blackSecondary-400
        "
        :type="type"
        :placeholder="placeholder"
        :min="minInput"
        :minlength="minlength"
        :maxlength="maxlength"
        :value="value"
        @input="updateValue"
      />
    </div>
  </div>
</template>
<script>
export default {
  props: [
    "placeholder",
    "type",
    "value",
    "errorOccured",
    "minInput",
    "maxlength",
    "minlength",
    "name",
  ],
  methods: {
    updateValue(event) {
      let currentValue = event.target.value;
      if (this.name === "phone" || this.type === "number") {
        currentValue = currentValue.replace(/\D/g, "");
      }

      this.$emit("input", currentValue);
    },
  },
};
</script>