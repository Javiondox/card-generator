<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title> Creador de cartas </q-toolbar-title>

        <div>Hecho por Javiondox</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <div class="q-pa-md q-gutter-sm">
        <textarea
          v-model="markdown"
          :rows="10"
          class="fit q-pa-sm"
          placeholder="Escribe el contenido..."
        />
        <div
          :style="{
            display: 'flex',
            justifyContent: 'start',
            padding: 0,
            width: width,
          }"
        >
          <CardComponent
            :text="markdown"
            :color="color"
            :tcolor="tcolor"
            :index="'0'"
          />
        </div>
        <q-btn color="primary w-100" @click="createCard">Nueva carta</q-btn>
        <q-btn color="primary w-100" @click="saveCard">Guardar carta</q-btn>
        <q-input label="Color del fondo" v-model="color" />
        <q-input label="Color del texto" v-model="tcolor" />
        <a href="https://quasar.dev/style/color-palette#color-list"
          >Puedes usar estos colores</a
        >
        <div class="w-100">
          <q-file v-model="file" label="Importar" @input="handleFileUpload">
            <template v-slot:prepend>
              <q-icon name="attach_file" />
            </template>
          </q-file>
        </div>
        <q-btn color="primary w-100" @click="handleExport"
          >Descargar cartas</q-btn
        >
        <q-separator />
        <b>Afecta a todas las cartas:</b>
        <q-input label="Ancho" v-model="width" />
        <q-input label="Alto" v-model="height" />
        <q-input label="Cada cuando saltar de página" v-model="pagebreaks" />
        <b>Tips:</b>
        <p>Puedes poner fotos poniendo ![](URL)</p>
        <p>Alternativamente, &lt;img src="URL" width="xx" height="xx"/&gt;</p>
        <p>&lt;br/&gt;, para saltos de línea</p>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref, provide } from "vue";
import "@quasar/quasar-ui-qmarkdown/dist/index.css";
import CardComponent from "src/components/CardComponent.vue";
import download from "downloadjs";

export default defineComponent({
  name: "MainLayout",

  components: {
    CardComponent,
  },

  setup() {
    const leftDrawerOpen = ref(false),
      splitterModel = ref(50),
      file = ref(),
      color = ref("white"),
      tcolor = ref("black"),
      height = ref("88mm"),
      width = ref("63mm"),
      pagebreaks = ref("4"),
      cardsArray = ref([]),
      selectedFile = ref(null),
      fileBlob = ref(new Blob()),
      fileData = ref(""),
      markdown = ref("");

    provide("markdownContent", markdown);
    provide("cardHeight", height);
    provide("cardWidth", width);
    provide("pagebreak", pagebreaks);
    provide("cardsArray", cardsArray);

    const uniqueId = () => parseInt(Date.now() * Math.random()).toString();

    function createCard() {
      markdown.value = "";
    }

    function saveCard() {
      cardsArray.value.push({
        id: uniqueId(),
        text: markdown.value,
        color: color.value,
        tcolor: tcolor.value,
      });
    }

    function exportCards() {}

    function importCards() {}

    function handleFileUpload(e) {
      selectedFile.value = fileBlob.value = e.target.files[0];
      console.log(selectedFile.value);
      if (selectedFile.value.type == "application/json") getFileData();
    }

    const getFileData = () => {
      const reader = new FileReader();

      reader.onload = () => {
        //Sobrecargamos la carga para que lo añada de forma asincrona al contenido
        fileData.value = reader.result;
        handleImport();
      };

      reader.readAsText(selectedFile.value); //Carga como texto
    };

    const handleImport = async () => {
      //$q.loading.show();
      try {
        let blob = fileBlob.value;
        cardsArray.value = JSON.parse(await blob.text());
        console.log(cardsArray.value);
        //$q.notify({ type: "positive", message: $t("settings.sync.imported") });
      } catch (e) {
        console.error(e);
        //$q.notify({ type: "negative", message: $t("settings.sync.invalidDB") });
      }
      //$q.loading.hide();
    };

    const handleExport = async () => {
      //$q.loading.show();
      const blob = new Blob([JSON.stringify(cardsArray.value, "null", "\t")], {
        type: "application/json", // or whatever your Content-Type is
      });
      //$q.loading.hide();
      download(blob, "cartas.json", "application/json");
      //$q.notify({ type: "positive", message: $t("settings.sync.exported") });
    };

    return {
      leftDrawerOpen,
      splitterModel,
      markdown,
      createCard,
      exportCards,
      importCards,
      saveCard,
      file,
      color,
      height,
      width,
      tcolor,
      pagebreaks,
      cardsArray,
      handleFileUpload,
      getFileData,
      handleImport,
      handleExport,
      selectedFile,
      fileBlob,
    };
  },
});
</script>

<style scoped>
.w-100 {
  width: 100%;
}
</style>
