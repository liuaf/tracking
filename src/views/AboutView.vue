<template>
  <div
    class="about"
    id="demo"
    ref="demo"
    style="width: 1282px; height: 635px"
  ></div>
</template>
<script>
import "ol/ol.css";
import Map from "ol/Map.js";
import OSM from "ol/source/OSM.js";
import View from "ol/View.js";
import ImageLayer from "ol/layer/Image";
import { ImageStatic } from "ol/source";
import { Projection } from "ol/proj";
import Static from "ol/source/ImageStatic";
import { getCenter } from "ol/extent.js";

import { Feature } from "ol";
import { LineString } from "ol/geom";
import Point from "ol/geom/Point.js";
import { Icon, Style } from "ol/style.js";
import { OGCMapTile, Vector as VectorSource } from "ol/source.js";
import { Tile as TileLayer, Vector as VectorLayer } from "ol/layer.js";
import { fromLonLat } from "ol/proj.js";
export default {
  name: "",
  data() {
    return {
      map: null,
      imgx: 800,
      imgy: 600,
      imageSource: null,
      lineFeature: null,
      // 图标集合
      featureList: [],
      deviceList: [
        {
          id: 282,
          mapLat: 499.0072821235625,
          mapLon: 263.3565891472875,
          name: "5区与1区中间靠近广场-人脸",
        },
        {
          id: 283,
          mapLat: 681.2036645525035,
          mapLon: 152.68357998590596,
          name: "面对机房-人脸",
        },
        {
          id: 284,
          mapLat: 388.48484848484946,
          mapLon: 263.80831571529313,
          name: "1区与5区中间位置-人脸",
        },
        {
          id: 285,
          mapLat: 733.6039464411576,
          mapLon: 279.16701902748486,
          name: "4区与2区中间-人脸",
        },
        {
          id: 286,
          mapLat: 250.85605283339666,
          mapLon: 315.7867647058817,
          name: "不止奶咖面对小黄鱼-人脸",
        },
        {
          id: 287,
          mapLat: 522.1959126145186,
          mapLon: 288.20155038759765,
          name: "弗蓝咖面对1区-人脸",
        },
        {
          id: 288,
          mapLat: 684.8174770965486,
          mapLon: 302.65680056377806,
          name: "2区民俗绝技位置-人脸",
        },
        {
          id: 289,
          mapLat: 655.9252450980379,
          mapLon: 303.2181372549013,
          name: "2区广场1-人脸",
        },
        {
          id: 290,
          mapLat: 807.6871035940824,
          mapLon: 320.72586328400365,
          name: "2区东侧-人脸",
        },
        {
          id: 291,
          mapLat: 879.9633544749846,
          mapLon: 197.85623678646985,
          name: "3区-人脸",
        },
        {
          id: 292,
          mapLat: 880.2645055203218,
          mapLon: 234.89781536293225,
          name: "3区入口-人脸",
        },
        {
          id: 293,
          mapLat: 710.7164669955384,
          mapLon: 138.52948085506262,
          name: "7区机房位置-人脸",
        },
        {
          id: 294,
          mapLat: 253.87033121916906,
          mapLon: 304.4637068358007,
          name: "8区园区入口-人脸",
        },
        {
          id: 295,
          mapLat: 302.65680056377806,
          mapLon: 252.9668780831578,
          name: "8区凤舞大串位置-人脸",
        },
        {
          id: 296,
          mapLat: 564.6582100070486,
          mapLon: 230.38054968287585,
          name: "广场蹦床位置-人脸",
        },
        {
          id: 297,
          mapLat: 420.1057082452442,
          mapLon: 168.0422832980977,
          name: "6区卷个凉皮位置-人脸",
        },
        {
          id: 298,
          mapLat: 446.60700023490836,
          mapLon: 132.80761099365785,
          name: "6区与5区连桥1层位置-人脸",
        },
        {
          id: 299,
          mapLat: 789.6180408738568,
          mapLon: 275.55320648343974,
          name: "4区面对人间大炮位置-人脸",
        },
        {
          id: 300,
          mapLat: 812.2043692741388,
          mapLon: 268.32558139534956,
          name: "2区面对4号入口-人脸",
        },
        {
          id: 301,
          mapLat: 184.30443974630066,
          mapLon: 332.47075405215026,
          name: "1号入口广场2-人脸",
        },
        {
          id: 353,
          mapLat: 672.1691331923907,
          mapLon: 250.25651867512394,
          name: "2区面对甜过初恋-人脸",
        },
        {
          id: 361,
          mapLat: 745.3488372093042,
          mapLon: 335.18111346018406,
          name: "105-人脸2",
        },
        {
          id: 362,
          mapLat: 760.5569649988277,
          mapLon: 315.00399342259885,
          name: "105-人脸1",
        },
        {
          id: 363,
          mapLat: 708.3072586328418,
          mapLon: 245.73925299506755,
          name: "104-人脸1",
        },
        {
          id: 364,
          mapLat: 700.1761804087403,
          mapLon: 266.51867512332694,
          name: "104-人脸2",
        },
        {
          id: 365,
          mapLat: 480.6370683579998,
          mapLon: 131.0007047216353,
          name: "103-人脸1",
        },
        {
          id: 366,
          mapLat: 486.96124031007884,
          mapLon: 163.5250176180413,
          name: "103-人脸2",
        },
        {
          id: 367,
          mapLat: 465.2783650458081,
          mapLon: 243.93234672304501,
          name: "102-人脸1",
        },
        {
          id: 368,
          mapLat: 471.00023490721287,
          mapLon: 278.2635658914736,
          name: "102-人脸2",
        },
        {
          id: 369,
          mapLat: 538.4580690627218,
          mapLon: 305.668311017149,
          name: "101-人脸1",
        },
        {
          id: 370,
          mapLat: 508.6441155743494,
          mapLon: 224.9598308668082,
          name: "107-人脸1",
        },
        {
          id: 371,
          mapLat: 494.18886539816896,
          mapLon: 226.76673713883076,
          name: "107-人脸2",
        },
        {
          id: 375,
          mapLat: 732.7004933051463,
          mapLon: 174.3664552501766,
          name: "106-人脸1",
        },
        {
          id: 376,
          mapLat: 745.3488372093042,
          mapLon: 149.06976744186085,
          name: "106-人脸2",
        },
      ],
    };
  },
  mounted() {
    this.initMap();
    window.addEventListener("resize", this.handleResize);
  },
  beforeDestroy() {
    // 清理事件监听器
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    handleResize() {
      // 当窗口大小变化时，重新初始化地图
      this.initMap();
    },
    initMap() {
      this.imgx = this.$refs.demo.offsetWidth;
      this.imgy = this.$refs.demo.offsetHeight;
      console.log(this.imgx, this.imgy);
      const extent = [0, 0, 1282, 635];
      const projection = new Projection({
        code: "xkcd-image",
        units: "pixels",
        extent: extent,
      });
      // 创建一个 Feature 对象来表示线段
      this.lineFeature = new Feature({
        geometry: new LineString([
          [0, 0],
          [100, 200],
        ]),
      });
      this.deviceList.forEach((d) => {
        const iconFeature = new Feature({
          geometry: new Point([ d.mapLat, - d.mapLon + 650]),
          name: "Null Island",
          population: 100,
          rainfall: 500,
        });

        const iconStyle = new Style({
          image: new Icon({
            anchor: [0.5, 46],
            anchorXUnits: "fraction",
            anchorYUnits: "pixels",
            src: "1.png",
            width: "20",
            height: "20",
          }),
        });

        iconFeature.setStyle(iconStyle);
        this.featureList.push(iconFeature);
      });

      this.map = new Map({
        target: "demo",
        layers: [
          // new TileLayer({
          //   source: new OSM(),
          // }),
          new ImageLayer({
            source: new Static({
              attributions:
                '© <a href="https://xkcd.com/license.html">xkcd</a>',
              url: require("../assets/map.jpg"),
              projection,
              imageExtent: extent,
            }),
          }),
          new VectorLayer({
            source: new VectorSource({
              features: this.featureList,
            }),
          }),
        ],
        view: new View({
          projection: projection,
          center: getCenter(extent),
          zoom: 2,
          maxZoom: 8,
        }),
      });
    },
  },
};
</script>
