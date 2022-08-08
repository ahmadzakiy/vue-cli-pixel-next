<template>
  <pixel-wrapper>
    <mp-flex
      min-height="100vh"
      p="12"
      align-items="center"
      justify-content="center"
      direction="column"
      bg="gray.50"
    >
      <mp-heading as="h1" font-size="2xl" mb="8">
        Pixel with Drag and Drop
      </mp-heading>
      <mp-flex width="600px" bg="white" p="4" direction="column">
        <mp-text font-weight="semibold">List content type</mp-text>
        <mp-text color="gray.600" mb="2">
          You can add a minimum of 1 content and add up to 10 contents.
        </mp-text>
        <mp-box
          ><mp-button
            left-icon="add-circular"
            variant="outline"
            mb="3"
            @click="handleAdd"
          >
            Add list
          </mp-button>
        </mp-box>

        <mp-flex width="full">
          <Draggable
            v-model="list"
            v-bind="dragOptions"
            handle=".handle"
            style="width: 100%"
            @start="drag = true"
            @end="drag = false"
          >
            <TransitionGroup type="transition" :name="!drag ? 'list' : null">
              <ListItem
                v-for="val in list"
                :key="val.id"
                :id="val.id"
                :value="val.name"
                @remove="handleRemove"
                @input="handleInput"
              />
            </TransitionGroup>
          </Draggable>
        </mp-flex>
      </mp-flex>

      <mp-flex width="600px" bg="white" p="4" mt="10" direction="column">
        <mp-text font-weight="semibold">Raw Data</mp-text>
        <mp-text as="pre" color="gray.600" mb="2" style="text-align: start">
          {{ prettyList }}
        </mp-text>
      </mp-flex>
    </mp-flex>
  </pixel-wrapper>
</template>

<script>
import PixelWrapper from "./components/PixelWrapper";
import ListItem from "./components/ListItem.vue";
import Draggable from "vuedraggable";

import { MpHeading, MpText, MpButton, MpFlex, MpBox } from "@mekari/pixel";

export default {
  name: "App",
  components: {
    PixelWrapper,
    MpHeading,
    MpText,
    MpButton,
    MpFlex,
    MpBox,
    ListItem,
    Draggable,
  },
  data() {
    return {
      drag: false,
      list: [
        {
          id: 1,
          name: "WA Order",
        },
        {
          id: 2,
          name: "Cek Status Order",
        },
        {
          id: 3,
          name: "Informasi Umum",
        },
        {
          id: 4,
          name: "Saran & Keluhan",
        },
      ],
    };
  },
  computed: {
    prettyList() {
      return JSON.stringify(this.list, null, 8);
    },

    dragOptions() {
      return {
        animation: 200,
        disabled: false,
        ghostClass: "ghost",
      };
    },
  },
  methods: {
    handleAdd() {
      this.list.push({
        name: "",
        id: this.randomId(),
      });
    },
    handleRemove(id) {
      this.list = this.list.filter((item) => item.id !== id);
    },
    handleInput(id, val) {
      this.list.map((item) => {
        if (item.id === id) {
          item.name = val;
        }
      });
    },
    randomId() {
      return Math.floor(Math.random() * 100) + 1;
    },
    handleDrag(e) {
      console.log("drag start", e);
    },
  },
};
</script>

<style>
.ghost {
  opacity: 0;
}
</style>
