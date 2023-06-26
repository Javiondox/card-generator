<template>
  <div>
    <q-card
      class="q-card"
      :style="{
        width: width,
        height: height,
        background: color,
        color: tcolor,
        padding: '1.25rem',
        position: 'relative',
      }"
    >
      <q-btn
        round
        color="red"
        v-if="id != undefined"
        class="closeButton"
        @click="deleteCard"
        >X</q-btn
      >
      <q-markdown v-bind:src="text" :no-heading-anchor-links="true">
      </q-markdown>
    </q-card>
    <PageBreak v-if="pagebreak % index" />
    <div v-if="pagebreak % index">AA</div>
  </div>
</template>

<script>
import { defineComponent, inject } from "vue";
import { QMarkdown } from "@quasar/quasar-ui-qmarkdown";
export default defineComponent({
  name: "CardComponent",
  props: ["text", "color", "tcolor", "id"],
  components: {
    QMarkdown,
  },
  setup(props) {
    const height = inject("cardHeight");
    const width = inject("cardWidth");
    const cardArray = inject("cardsArray");

    function deleteCard() {
      cardArray.value = cardArray.value.filter((card) => card.id != props.id);
    }

    return {
      height,
      width,
      cardArray,
      deleteCard,
    };
  },
});
</script>

<style scoped>
.q-markdown pre,
.q-markdown code {
  background: white !important;
}

.q-card:hover .closeButton {
  display: flex;
  position: absolute;
  right: -25px;
}
.closeButton {
  display: none;
}
</style>
