<template>
  <mp-flex align="center" width="full" :id="id" mb="3">
    <mp-icon name="drag" cursor="move" class="handle" />
    <mp-input-group mx="2">
      <mp-input
        v-model="currentValue"
        maxlength="60"
        placeholder="Enter item name"
        @input="handleInput"
      />
      <mp-input-right-addon :with-background="false">
        <mp-text color="gray.400">
          {{ handleCountChar(value.length) }}
        </mp-text>
      </mp-input-right-addon>
    </mp-input-group>
    <mp-button-icon name="minus-circular" @click="handleRemove" />
  </mp-flex>
</template>

<script>
import {
  MpFlex,
  MpIcon,
  MpText,
  MpInput,
  MpInputGroup,
  MpInputRightAddon,
  MpButtonIcon,
} from "@mekari/pixel";

export default {
  name: "ListItem",
  components: {
    MpFlex,
    MpIcon,
    MpText,
    MpInput,
    MpInputGroup,
    MpInputRightAddon,
    MpButtonIcon,
  },
  props: {
    id: {
      type: Number,
      default: 0,
    },
    value: {
      type: String,
      default: "asd",
    },
  },
  data() {
    return {
      currentValue: this.value,
    };
  },
  watch: {
    value(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.currentValue = newVal;
      }
    },
  },
  methods: {
    handleCountChar(number) {
      let counter = 60 - number;
      return counter + "/60";
    },
    handleRemove() {
      this.$emit("remove", this.id);
    },
    handleInput() {
      this.$emit("input", this.id, this.currentValue);
    },
  },
};
</script>
