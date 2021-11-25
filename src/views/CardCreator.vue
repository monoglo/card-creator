<template>
  <div class="ma-4">
    <v-row>
      <v-col cols="12" :xs="ratio == 1.25 ? 4 : 3" :lg="ratio == 1.25 ? 4 : 3">
        <!-- 卡牌预览 -->
        <v-card
          max-width="248"
          height="352"
          class="text-center"
          :color="cardBackgroundColor"
          id="card"
        >
          <v-card-title class="text-center" :style="'color: ' + cardTitleColor">
            {{ cardTitle }}
          </v-card-title>
          <div style="height: 180px">
            <vue-cropper
              ref="cropper"
              :img="imageUrl"
              :outputSize="1"
              outputType="png"
            ></vue-cropper>
            <!-- <img :src="resultImageUrl" style="width: 100; height: 100%" /> -->
          </div>

          <v-card-subtitle
            :style="
              'height: 72px;padding: 0;display: flex;justify-content: center;align-items: center;color:' +
              cardSubTitleColor
            "
          >
            {{ cardSubTitle }}
          </v-card-subtitle>
          <div style="width: 100%">
            <v-divider></v-divider>
            <v-btn
              text
              class="font-weight-black"
              :style="'color: ' + cardFooterColor"
            >
              {{ cardType }}
            </v-btn>
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
          <v-text-field v-model="cardTitle" :label="$t('form.cardTitle')">
            <v-menu
              offset-y
              slot="append"
              :close-on-content-click="false"
              offset-x
            >
              <template v-slot:activator="{ on, attrs }">
                <v-icon
                  slot="append"
                  :color="cardTitleColor"
                  v-bind="attrs"
                  v-on="on"
                >
                  mdi-format-color-text
                </v-icon>
              </template>
              <v-color-picker
                v-model="cardTitleColor"
                dot-size="25"
                swatches-max-height="200"
                show-swatches
                @input="onColorChange"
              ></v-color-picker>
            </v-menu>
          </v-text-field>
          <v-file-input
            truncate-length="15"
            accept="image/*"
            :label="$t('form.uploadImageFile')"
            @change="uploadImage"
          >
            <v-menu
              offset-y
              slot="append"
              :close-on-content-click="false"
              offset-x
            >
              <template v-slot:activator="{ on, attrs }">
                <v-icon
                  slot="append"
                  :color="cardBackgroundColor"
                  v-bind="attrs"
                  v-on="on"
                >
                  mdi-format-color-fill
                </v-icon>
              </template>
              <v-color-picker
                v-model="cardBackgroundColor"
                dot-size="25"
                swatches-max-height="200"
                show-swatches
              ></v-color-picker>
            </v-menu>
          </v-file-input>
          <v-text-field v-model="cardSubTitle" :label="$t('form.cardSubtitle')">
            <v-menu
              offset-y
              slot="append"
              :close-on-content-click="false"
              offset-x
            >
              <template v-slot:activator="{ on, attrs }">
                <v-icon
                  slot="append"
                  :color="cardSubTitleColor"
                  v-bind="attrs"
                  v-on="on"
                >
                  mdi-format-color-text
                </v-icon>
              </template>
              <v-color-picker
                v-model="cardSubTitleColor"
                dot-size="25"
                swatches-max-height="200"
                show-swatches
                @input="onColorChange"
              ></v-color-picker>
            </v-menu>
          </v-text-field>
          <v-text-field v-model="cardType" :label="$t('form.cardType')">
            <v-menu
              offset-y
              slot="append"
              :close-on-content-click="false"
              offset-x
            >
              <template v-slot:activator="{ on, attrs }">
                <v-icon
                  slot="append"
                  :color="cardFooterColor"
                  v-bind="attrs"
                  v-on="on"
                >
                  mdi-format-color-text
                </v-icon>
              </template>
              <v-color-picker
                v-model="cardFooterColor"
                dot-size="25"
                swatches-max-height="200"
                show-swatches
                @input="onColorChange"
              ></v-color-picker>
            </v-menu>
          </v-text-field>
          <v-switch
            v-model="syncFontColor"
            :label="$t('form.syncFontColor')"
            @change="onSyncChange"
          ></v-switch>
          <v-row>
            <v-col cols="8"> </v-col>
            <v-col cols="4">
              <v-btn
                class="font-weight-black"
                @click="exportCanvas"
                color="primary"
                style="margin-top: 160px"
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
        :xs="ratio == 1.25 ? 5 : 6"
        :lg="ratio == 1.25 ? 5 : 6"
        id="result"
        style="zoom: 175%; display: flex; overflow-x: auto; white-space: nowrap"
      >
      </v-col>
    </v-row>
    <v-divider></v-divider>
    justify image in left preview, then click "EXPORT" and then right click to
    save image <br />
    拖拽左侧预览中的图片到合适位置，然后点击 “生成” 然后右键保存图片
  </div>
</template>

<script>
import html2canvas from "html2canvas";
import { VueCropper } from "vue-cropper";

export default {
  components: {
    "vue-cropper": VueCropper,
  },
  data() {
    return {
      cardBackgroundColor: "#FFB74D",
      cardTitleColor: "#311B92",
      cardSubTitleColor: "#21180a",
      cardFooterColor: "#21180a",
      cardSubTitleColorBackup: "#21180a",
      cardFooterColorBackup: "#21180a",
      cardTitle: this.$t("sample.title"),
      cardSubTitle: this.$t("sample.detail"),
      cardType: this.$t("sample.type"),
      imageUrl: "/sunset-3988885_1280.jpg",
      scale: 3,
      ratio: 1,
      syncFontColor: false,
    };
  },
  mounted() {
    this.ratio = window.devicePixelRatio;
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
      this.imageUrl = URL.createObjectURL(files);
    },
    onColorChange(color) {
      if (this.syncFontColor) {
        this.cardTitleColor = color;
        this.cardSubTitleColor = color;
        this.cardFooterColor = color;
      }
    },
    onSyncChange(sync) {
      if (sync) {
        this.cardSubTitleColorBackup = this.cardSubTitleColor;
        this.cardFooterColorBackup = this.cardFooterColor;
        this.cardSubTitleColor = this.cardTitleColor;
        this.cardFooterColor = this.cardTitleColor;
      } else {
        this.cardSubTitleColor = this.cardSubTitleColorBackup;
        this.cardFooterColor = this.cardFooterColorBackup;
      }
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
#result {
  padding: 0 0 0 12px;
}
</style>
