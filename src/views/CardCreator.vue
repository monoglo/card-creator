<template>
  <div class="my-4">
    <v-row dense>
      <v-col cols="12" :xs="ratio == 1.25 ? 4 : 3" :lg="ratio == 1.25 ? 4 : 3">
        <!-- 卡牌预览 -->
        <v-card flat tile>
          <div style="display: flex; justify-content: space-around">
            <v-window v-model="onboarding" id="card">
              <v-window-item>
                <v-card
                  width="248"
                  height="352"
                  class="text-center"
                  :color="cardBackgroundColor"
                  :style="'font-family: ' + selectedFont"
                >
                  <v-card-title
                    :style="
                      'color: ' +
                      cardTitleColor +
                      ';justify-content: ' +
                      titleJustify
                    "
                  >
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
              </v-window-item>
              <!-- 模板2 -->
              <v-window-item>
                <v-card
                  width="248"
                  height="352"
                  class="text-center"
                  :color="cardBackgroundColor"
                  :style="'font-family: ' + selectedFont"
                >
                  <div style="height: 180px; overflow: hidden">
                    <vue-cropper
                      ref="cropper"
                      :img="imageUrl"
                      :outputSize="1"
                      outputType="png"
                    ></vue-cropper>
                  </div>
                  <v-card-title
                    :style="
                      'color: ' +
                      cardTitleColor +
                      ';justify-content: ' +
                      titleJustify +
                      ';padding: 4px 16px'
                    "
                  >
                    {{ cardTitle }}
                  </v-card-title>
                  <v-divider></v-divider>
                  <v-card-subtitle
                    :style="
                      'height: 94px;padding: 0;display: flex;justify-content: center;align-items: center;color:' +
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
              </v-window-item>
            </v-window>
          </div>
          <v-card-actions class="justify-space-between">
            <v-btn text @click="prev">
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn>
            <v-item-group v-model="onboarding" class="text-center" mandatory>
              <v-item
                v-for="n in length"
                :key="`btn-${n}`"
                v-slot="{ active, toggle }"
              >
                <v-btn :input-value="active" icon @click="toggle">
                  <v-icon>mdi-record</v-icon>
                </v-btn>
              </v-item>
            </v-item-group>
            <v-btn text @click="next">
              <v-icon>mdi-chevron-right</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-col cols="11" xs="3" lg="3" id="right">
        <v-card color="">
          <v-card-text>
            <v-form>
              <!-- TODO 导出分辨率 -->
              <!-- <v-slider
            hint="Im a hint"
            max="3"
            min="1"
            v-model="scale"
            :label="$t('form.exportScale')"
            :tick-labels="[$t('form.low'), $t('form.medium'), $t('form.high')]"
          ></v-slider> -->
              <v-text-field v-model="cardTitle" :label="$t('form.cardTitle')">
                <v-icon
                  slot="append"
                  :color="titleJustify == 'left' ? 'primary' : 'black'"
                  @click="switchTitleJustifyToLeft"
                >
                  mdi-format-align-left
                </v-icon>
                <v-icon
                  slot="append"
                  :color="titleJustify == 'center' ? 'primary' : 'black'"
                  @click="switchTitleJustifyToCenter"
                >
                  mdi-format-align-center
                </v-icon>
                <v-icon
                  slot="append"
                  :color="titleJustify == 'right' ? 'primary' : 'black'"
                  @click="switchTitleJustifyToRight"
                >
                  mdi-format-align-right
                </v-icon>
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
              <v-text-field
                v-model="cardSubTitle"
                :label="$t('form.cardSubtitle')"
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
              <v-combobox
                v-model="selectedFont"
                :items="fonts"
                dense
                :label="$t('form.cardFont')"
              >
                <template v-slot:item="{ item, attrs, on }">
                  <v-list-item v-on="on" v-bind="attrs">
                    <v-list-item-content>
                      <v-list-item-title :style="'font-family: ' + item">
                        {{ item }}
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </template>
              </v-combobox>
              <v-row>
                <v-col cols="8"> </v-col>
                <v-col cols="4">
                  <v-switch
                    v-model="show3D"
                    :label="$t('form.show3D')"
                    style="margin-top: 100px"
                  ></v-switch>
                  <v-btn
                    class="font-weight-black"
                    @click="exportCanvas"
                    color="primary"
                    large
                  >
                    {{ $t("form.export") }}
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col
        cols="12"
        :xs="ratio == 1.25 ? 5 : 6"
        :lg="ratio == 1.25 ? 5 : 6"
        style="
          display: flex;
          overflow-x: auto;
          white-space: nowrap;
          height: 657px;
        "
      >
        <div class="l-container" v-show="show3D">
          <div
            class="b-game-card"
            v-for="item in exportCanvasList"
            v-bind:key="item"
          >
            <div class="cover" :style="'background-image: url(' + item + ');'">
              <div class="gloss"></div>
            </div>
          </div>
        </div>
        <div id="result" v-show="!show3D" style="zoom: 175%"></div>
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
import $ from "jquery";

export default {
  components: {
    "vue-cropper": VueCropper,
  },
  watch: {
    onboarding(onboarding) {
      if (onboarding == 1) {
        this.titleJustify = "center";
      }
    },
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
      fonts: [],
      selectedFont: "'Roboto', sans-serif",
      onboarding: 0,
      length: 2,
      titleJustify: "left",
      exportCanvasList: [],
      show3D: false,
    };
  },
  mounted() {
    this.ratio = window.devicePixelRatio;
    this.listFonts();
  },
  methods: {
    switchTitleJustifyToCenter() {
      this.titleJustify = "center";
    },
    switchTitleJustifyToLeft() {
      this.titleJustify = "left";
    },
    switchTitleJustifyToRight() {
      this.titleJustify = "right";
    },
    exportCanvas() {
      html2canvas(document.getElementById("card"), {
        scale: this.scale,
        allowTaint: true,
        useCORS: true,
      }).then((canvas) => {
        document.getElementById("result").prepend(canvas);
        this.exportCanvasList.unshift(canvas.toDataURL());
        setTimeout(() => {
          const maxTilt = 30;
          $(".b-game-card").unbind();
          $(".b-game-card")
            .mousemove(function (evt) {
              let bounding = mouseOverBoundingElem(evt);

              let posX = bounding.width / 2 - bounding.x;
              let posY = bounding.height / 2 - bounding.y;
              let hypotenuseCursor = Math.sqrt(
                Math.pow(posX, 2) + Math.pow(posY, 2)
              );
              let hypotenuseMax = Math.sqrt(
                Math.pow(bounding.width / 2, 2) +
                  Math.pow(bounding.height / 2, 2)
              );
              let ratio = hypotenuseCursor / hypotenuseMax;

              $(".cover", this).css({
                transform: `rotate3d(${posY / hypotenuseCursor}, ${
                  -posX / hypotenuseCursor
                }, 0, ${ratio * maxTilt}deg)`,
                filter: `brightness(${1.6 - bounding.y / bounding.height})`, // 0.6 = offset, brightness will be from 0.6 to 1.6
              });
              $(".gloss", this).css({
                transform: `translateX(${posX * ratio * 0.75}px) translateY(${
                  posY * ratio
                }px)`, // 0.75 = offset
              });
            })
            .mouseleave(function () {
              let css = {
                transform: "",
                filter: "",
              };
              $(".cover, .gloss", this).css(css);
            });

          function mouseOverBoundingElem(evt) {
            let bounding = evt.target.getBoundingClientRect();
            let x = evt.originalEvent.pageX - Math.round(bounding.left);
            let y = evt.originalEvent.pageY - Math.round(bounding.top);

            return {
              x: Math.max(0, x),
              y: Math.max(0, y),
              width: Math.round(bounding.width),
              height: Math.round(bounding.height),
            };
          }
        }, 300);
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
    next() {
      this.onboarding =
        this.onboarding + 1 === this.length ? 0 : this.onboarding + 1;
    },
    prev() {
      this.onboarding =
        this.onboarding - 1 < 0 ? this.length - 1 : this.onboarding - 1;
    },
    listFonts() {
      const fontCheck = new Set(
        [
          // Windows 10
          "Arial",
          "Arial Black",
          "Bahnschrift",
          "Calibri",
          "Cambria",
          "Cambria Math",
          "Candara",
          "Comic Sans MS",
          "Consolas",
          "Constantia",
          "Corbel",
          "Courier New",
          "Ebrima",
          "Franklin Gothic Medium",
          "Gabriola",
          "Gadugi",
          "Georgia",
          "HoloLens MDL2 Assets",
          "Impact",
          "Ink Free",
          "Javanese Text",
          "Leelawadee UI",
          "Lucida Console",
          "Lucida Sans Unicode",
          "Malgun Gothic",
          "Marlett",
          "Microsoft Himalaya",
          "Microsoft JhengHei",
          "Microsoft New Tai Lue",
          "Microsoft PhagsPa",
          "Microsoft Sans Serif",
          "Microsoft Tai Le",
          "Microsoft YaHei",
          "Microsoft Yi Baiti",
          "MingLiU-ExtB",
          "Mongolian Baiti",
          "MS Gothic",
          "MV Boli",
          "Myanmar Text",
          "Nirmala UI",
          "Palatino Linotype",
          "Segoe MDL2 Assets",
          "Segoe Print",
          "Segoe Script",
          "Segoe UI",
          "Segoe UI Historic",
          "Segoe UI Emoji",
          "Segoe UI Symbol",
          "SimSun",
          "Sitka",
          "Sylfaen",
          "Symbol",
          "Tahoma",
          "Times New Roman",
          "Trebuchet MS",
          "Verdana",
          "Webdings",
          "Wingdings",
          "Yu Gothic",
          // macOS
          "American Typewriter",
          "Andale Mono",
          "Arial",
          "Arial Black",
          "Arial Narrow",
          "Arial Rounded MT Bold",
          "Arial Unicode MS",
          "Avenir",
          "Avenir Next",
          "Avenir Next Condensed",
          "Baskerville",
          "Big Caslon",
          "Bodoni 72",
          "Bodoni 72 Oldstyle",
          "Bodoni 72 Smallcaps",
          "Bradley Hand",
          "Brush Script MT",
          "Chalkboard",
          "Chalkboard SE",
          "Chalkduster",
          "Charter",
          "Cochin",
          "Comic Sans MS",
          "Copperplate",
          "Courier",
          "Courier New",
          "Didot",
          "DIN Alternate",
          "DIN Condensed",
          "Futura",
          "Geneva",
          "Georgia",
          "Gill Sans",
          "Helvetica",
          "Helvetica Neue",
          "Herculanum",
          "Hoefler Text",
          "Impact",
          "Lucida Grande",
          "Luminari",
          "Marker Felt",
          "Menlo",
          "Microsoft Sans Serif",
          "Monaco",
          "Noteworthy",
          "Optima",
          "Palatino",
          "Papyrus",
          "Phosphate",
          "Rockwell",
          "Savoye LET",
          "SignPainter",
          "Skia",
          "Snell Roundhand",
          "Tahoma",
          "Times",
          "Times New Roman",
          "Trattatello",
          "Trebuchet MS",
          "Verdana",
          "Zapfino",
          "宋体",
          "黑体",
          "微软雅黑",
          "微软雅黑",
          "微软正黑体",
          "新宋体",
          "新细明体",
          "细明体",
          "标楷体",
          "仿宋",
          "楷体",
          "仿宋_GB2312",
          "楷体_GB2312",
        ].sort()
      );

      (async () => {
        await document.fonts.ready;

        const fontAvailable = new Set();

        for (const font of fontCheck.values()) {
          if (document.fonts.check(`12px "${font}"`)) {
            fontAvailable.add(font);
          }
        }
        this.fonts = [...fontAvailable.values()];
      })();
    },
  },
};
</script>

<style lang="scss">
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
  // display: flex;
  // justify-content: space-around;
}
#result {
  padding: 0 0 0 12px;
}

.l-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 30px;
  width: 100%;
  max-width: 1200px;

  @media screen and (max-width: 760px) {
    grid-template-columns: repeat(2, 1fr);
  }
}

.b-game-card {
  position: relative;
  z-index: 1;
  height: 80%;
  width: 300px;
  padding-bottom: 150%;
  perspective: 1000px;
  transition: transform 0.35s;
  transform: scale(0.4);
  margin: -80px 0 0 0;

  .cover {
    border-radius: 11px;
    position: absolute;
    z-index: 1;
    top: 0;
    left: 0;
    width: 744px;
    height: 1056px;
    overflow: hidden;
    background-image: linear-gradient(120deg, #f6d365 0%, #fda085 100%);
    box-shadow: 0px 0px 8px 1px #000000ad;
    perspective-origin: 50% 50%;
    transform-style: preserve-3d;
    transform-origin: center;
    will-change: transform;
    transition: transform 0.5s, filter 0.35s;

    /* Gloss */
    .gloss {
      position: absolute;
      pointer-events: none;
      top: 0;
      left: 0;
      width: 20em;
      height: 20em;
      margin-left: -6em;
      margin-top: -17em;
      background: radial-gradient(
        circle farthest-corner at 50% 50%,
        rgba(255, 255, 255, 0.7) 0%,
        rgba(255, 255, 255, 0.5) 33%,
        rgba(255, 255, 255, 0) 60%,
        rgba(255, 255, 255, 0) 100%
      );
      opacity: 0;
      will-change: transform;
      transition: opacity 0.21s ease-in-out;
    }
  }

  &:hover {
    transform: scale(0.55);
    z-index: 10;

    .cover {
      box-shadow: 10px 20px 32px 1px #000000ab;
      transition: none;

      .gloss {
        opacity: 0.5;
      }
    }
  }
}
</style>
