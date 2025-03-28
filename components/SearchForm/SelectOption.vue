<template>
  <div class="relative w-full">
    <div ref="selectElement">
      <h2
        v-if="!isPassengerForm"
        :class="isRequired ? '' : 'justify-between'"
        class="text-xs lg:text-base font-medium text-blackPrimary flex items-end"
      >
        <span>{{ label }} </span>
        <span v-if="isOptional" class="text-[#8D8D8F] text-xs">Optional</span>
        <span v-if="isRequired" class="text-[#E0293B] ml-1">*</span>
      </h2>

      <div
        v-if="isPassengerForm"
        :class="isRequired && !isOptional ? '' : 'justify-between'"
        class="text-base lg:text-xl text-blackPrimary font-medium flex items-end"
      >
        <span>{{ label }} </span>
        <span v-if="isOptional" class="text-[#8D8D8F] text-xs">Optional</span>
        <span v-if="isRequired" class="text-[#E0293B] ml-1">*</span>
      </div>

      <button
        @click="toggleDropdown"
        :class="[
          'z-10 block bg-[#f7f7f7] px-4 w-full focus:outline-none',
          !isPassengerForm
            ? 'py-[13px] mt-[10px] rounded'
            : 'h-12 mt-2 rounded-lg',
          errorMessage && 'bg-[#FDF0F1] border border-[#E0293B]',
        ]"
      >
        <div class="flex justify-between items-center">
          <p
            class="text-blackPrimary text-sm font-normal"
            v-if="shouldDisplaySelectedOption"
          >
            {{ displaySelectedOption }}
          </p>
          <p class="text-blackPrimary text-sm font-normal" v-else>
            {{ defaultOption }}
          </p>
          <img src="@/assets/images/icons/dropdown.svg" alt="dropdown" />
        </div>
      </button>
    </div>

    <div
      v-if="errorMessage"
      class="w-full flex flex-row gap-x-2 items-center text-xs font-medium text-[#E0293B] mt-[10px]"
    >
      <img
        src="@/assets/images/icons/warningRed.svg"
        class="h-4 w-4"
        alt="error"
      />
      <div>Please select a {{ label.toLowerCase() }}.</div>
    </div>

    <div
      v-show="optionsIsOpen"
      class="border mt-4 w-11/12 bg-white rounded-md shadow-xl z-[1000] leading-6 before:grid absolute divide-y-2"
    >
      <div class="text-center p-4">
        <h2 class="font-inter text-xs xl:text-[16px] font-[600] text-td_text">
          <span v-if="defaultOption">{{ defaultOption }}</span>
          <span v-else>Select your Option</span>
        </h2>
      </div>
      <ul
        class="overflow-y-auto divide-y divide-dashed h-[344px] text-sm xl:text-md text-td_text px-4"
      >
        <li
          v-for="(option, index) in options"
          :key="index"
          class="cursor-pointer font-inter py-[14px] font-medium hover:text-corporate relative"
          :class="
            propertyName &&
            option[propertyName] === selectedOption[propertyName]
              ? 'text-corporate'
              : option === selectedOption
              ? 'text-corporate'
              : 'text-blackSecondary'
          "
          @click="selectOption(option)"
        >
          {{ propertyName ? option[propertyName] : option }}
          <span
            v-if="option === selectedOption"
            class="absolute right-5 top-5 bottom-0"
            ><img src="@/assets/images/icons/tik.svg" alt="" class="w-4 h-3"
          /></span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "SelectOption",
  model: {
    prop: "boardingPoint",
    event: "input",
  },
  props: {
    label: {
      type: String,
      required: false,
    },
    isPassengerForm: {
      type: Boolean,
      required: false,
    },
    defaultOption: {
      type: String,
      required: false,
    },
    options: {
      type: Array,
      required: true,
    },
    isOptional: {
      type: Boolean,
      required: false,
    },
    isRequired: {
      type: Boolean,
      required: false,
    },
    propertyName: "",
    boardingPoint: "",
    errorMessage: {
      type: Boolean,
      required: false,
    },
  },
  data() {
    return {
      optionsIsOpen: false,
      selectedOption: "",
    };
  },
  created() {
    this.selectedOption =
      this.options && this.options.length > 0 ? this.boardingPoint : "";
  },
  mounted() {
    window.addEventListener("click", this.close);
  },
  beforeDestroy() {
    window.removeEventListener("click", this.close);
  },
  methods: {
    toggleDropdown() {
      this.optionsIsOpen = !this.optionsIsOpen;
    },
    selectOption(option) {
      this.selectedOption = option;
      this.$emit("input", this.selectedOption);
      this.optionsIsOpen = false;
    },
    close(e) {
      if (!this.$el.contains(e.target)) {
        this.optionsIsOpen = false;
      }
    },
  },
  computed: {
    shouldDisplaySelectedOption() {
      return (
        this.selectedOption &&
        (this.selectedOption.name !== "" ||
          (typeof this.selectedOption !== "object" &&
            this.selectedOption !== ""))
      );
    },
    displaySelectedOption() {
      return this.propertyName
        ? this.selectedOption[this.propertyName]
        : this.selectedOption;
    },
  },
};
</script>
