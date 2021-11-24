<template>
  <div class="ma-6">
    <v-row>
      <v-col cols="12" xs="4" lg="4">
        <!-- 卡牌预览 -->
        <v-card
          max-width="248"
          height="352"
          class="text-center"
          :color="cardBackgroundColor"
          id="card"
        >
          <v-card-title class="text-center"> {{ cardTitle }} </v-card-title>
          <v-img :src="imageSrc" height="180px"></v-img>

          <v-card-subtitle
            style="
              height: 72px;
              padding: 0;
              display: flex;
              justify-content: center;
              align-items: center;
            "
          >
            {{ cardSubTitle }}
          </v-card-subtitle>
          <div style="width: 100%">
            <v-divider></v-divider>
            <v-btn text class="font-weight-black"> {{ cardType }} </v-btn>
          </div>
        </v-card>
      </v-col>
      <v-col cols="11" xs="3" lg="3" id="right"
        ><v-form>
          <v-slider
            hint="Im a hint"
            max="3"
            min="1"
            v-model="scale"
            :label="$t('form.exportScale')"
            :tick-labels="[$t('form.low'), $t('form.medium'), $t('form.high')]"
          ></v-slider>
          <v-text-field
            v-model="cardTitle"
            :label="$t('form.cardTitle')"
          ></v-text-field>
          <v-file-input
            truncate-length="15"
            accept="image/*"
            :label="$t('form.uploadImageFile')"
            @change="uploadImage"
          ></v-file-input>
          <v-text-field
            v-model="cardSubTitle"
            :label="$t('form.cardSubtitle')"
          ></v-text-field>
          <v-text-field
            v-model="cardType"
            :label="$t('form.cardType')"
          ></v-text-field>
          <v-row>
            <v-col cols="8">
              <v-color-picker
                v-model="cardBackgroundColor"
                dot-size="25"
                swatches-max-height="200"
              ></v-color-picker>
            </v-col>
            <v-col cols="4">
              <v-btn
                class="font-weight-black"
                @click="exportCanvas"
                color="primary"
                style="margin-top: 220px"
                large
              >
                {{ $t("form.export") }}
              </v-btn>
            </v-col>
          </v-row>
        </v-form>
      </v-col>
      <v-col
        cols="12"
        xs="5"
        lg="5"
        id="result"
        style="zoom: 175%; display: flex; overflow-x: auto; white-space: nowrap"
      >
      </v-col>
    </v-row>
    <v-divider></v-divider>
    click "EXPORT" and then right click to save image <br />
    点击 “生成” 然后右键保存图片
  </div>
</template>

<script>
import html2canvas from "html2canvas";

export default {
  data() {
    return {
      cardBackgroundColor: "#FFB74D",
      cardTitle: this.$t("sample.title"),
      cardSubTitle: this.$t("sample.detail"),
      cardType: this.$t("sample.type"),
      imageSrc: "/sunset-3988885_1280.jpg",
      scale: 3,
    };
  },
  methods: {
    exportCanvas() {
      html2canvas(document.getElementById("card"), {
        scale: this.scale,
        allowTaint: true,
        useCORS: true,
      }).then(function (canvas) {
        document.getElementById("result").prepend(canvas);
      });
    },
    uploadImage(files) {
      this.imageSrc = URL.createObjectURL(files);
    },
  },
};
</script>

<style>
canvas {
  margin-right: 2px;
}
@media screen and (-webkit-min-device-pixel-ratio: 0) and (min-resolution: 0.001dpcm) {
  #card {
    zoom: 175%;
  }
}
@-moz-document url-prefix() {
  #card {
    -moz-transform: scale(1.75);
    -moz-transform-origin: 0 0;
    -o-transform: scale(1.75);
    -o-transform-origin: 0 0;
    -webkit-transform: scale(1.75);
    -webkit-transform-origin: 0 0;
    transform: scale(1.75);
    transform-origin: 0 0;
  }
}
#card {
  box-shadow: none;
}
</style>
