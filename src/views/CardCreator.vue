<template>
  <div class="ma-6">
    <v-row>
      <v-col cols="3">
        <v-card
          max-width="248"
          height="352"
          class="text-center"
          :color="cardBackgroundColor"
          style="box-shadow: none; zoom: 175%"
          id="card"
        >
          <v-card-title class="text-center"> {{ cardTitle }} </v-card-title>
          <v-img :src="imageSrc" height="180px"></v-img>

          <v-card-subtitle style="margin: auto">
            {{ cardSubTitle }}
          </v-card-subtitle>
          <div style="position: absolute; bottom: 0; width: 100%">
            <v-divider></v-divider>
            <v-btn text class="font-weight-black"> {{ cardType }} </v-btn>
          </div>
        </v-card>
      </v-col>
      <v-col cols="9" id="right"
        ><v-form style="width: 400px">
          <v-btn
            class="font-weight-black"
            @click="exportCanvas"
            color="primary"
          >
            Export
          </v-btn>
          <v-slider
            hint="Im a hint"
            max="3"
            min="1"
            v-model="scale"
            label="导出分辨率"
            :tick-labels="['低', '中', '高']"
          ></v-slider>
          <v-text-field v-model="cardTitle" label="Card Title"></v-text-field>
          <v-file-input
            truncate-length="15"
            accept="image/*"
            label="Input Image File"
            @change="uploadImage"
          ></v-file-input>
          <v-text-field
            v-model="cardSubTitle"
            label="Card Subtitle"
          ></v-text-field>
          <v-text-field v-model="cardType" label="Card Type"></v-text-field>
          <v-color-picker
            v-model="cardBackgroundColor"
            dot-size="25"
            swatches-max-height="200"
          ></v-color-picker>
        </v-form>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" id="result" style="zoom: 175%"> </v-col>
    </v-row>
  </div>
</template>

<script>
import html2canvas from "html2canvas";

export default {
  data() {
    return {
      cardBackgroundColor: "#FFB74D",
      cardTitle: "龙骑士",
      cardSubTitle: "对抗龙系单位或与龙系单位协同作战时造成双倍伤害",
      cardType: "职业",
      imageSrc: "/sunshine.jpg",
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
        document.getElementById("result").appendChild(canvas);
      });
    },
    uploadImage(files) {
      this.imageSrc = URL.createObjectURL(files);
    },
  },
};
</script>

<style></style>
