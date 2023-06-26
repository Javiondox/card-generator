<template>
  <q-page class="cardGrid">
    <div v-for="(card, index) in cardArray" :key="card.id">
      <CardComponent
        :index="index"
        :id="card.id"
        :text="card.text"
        :tcolor="card.tcolor"
        :color="card.color"
      />
      <template v-if="(index + 1) % pagebreak === 0">
        <page-break />
      </template>
    </div>
  </q-page>
</template>

<script>
import PageBreak from "src/components/PageBreak.vue";
import CardComponent from "src/components/CardComponent.vue";
import { defineComponent, inject } from "vue";

export default defineComponent({
  name: "IndexPage",
  components: { CardComponent, PageBreak },
  setup() {
    const uniqueId = () => parseInt(Date.now() * Math.random()).toString();
    const pagebreak = inject("pagebreak");
    const cardArray = inject("cardsArray");
    const cards = [
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "black", tcolor: "white" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "red", tcolor: "white" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "blue", tcolor: "white" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "green", tcolor: "white" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
      { id: uniqueId(), text: "markdown", color: "white", tcolor: "black" },
    ];
    console.log("cards", cards);
    return { cards, pagebreak, cardArray };
  },
});
</script>

<style scoped>
.cardGrid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-left: 10px;
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>
