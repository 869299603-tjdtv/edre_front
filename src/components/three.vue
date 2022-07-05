<template>
<div id="container"></div>
</template>
<script>
// import { defineComponent } from '@vue/composition-api'
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { OrbitControls } from "../common/tool/OrbitControls";
import { CSS2DObject, CSS2DRenderer } from "../common/tool/CSS2DRenderer";
// import {settingsStore} from "/@/store/module/settings";
// import {IRoleList} from "/@/interface/role/list.interface";
// import {IHttpResult} from "/@/interface/common.interface";
// import { ILogListParams } from "/@/interface/settings/log.interface";
export default {
  name: "vueThree",
  setup() {},
  data() {
    return {
      /**场景 */
      scene: null,
      /**相机 */
      camera: null,
      /**渲染 */
      renderer: null,
      /**控制器 */
      controls: null,
      /**射线投射器 */
      raycaster: null,
      /**鼠标 */
      mouse: null,
      /**屋顶 */
      roof: null,
      /**组 */
      group: null,
      requestId: null,
      /**渲染文字 */
      CSSRender: null,
      /**容器 */
      container: null,
    };
  },
  methods: {
    animate() {
      // requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
      this.CSSRender.render(this.scene, this.camera);
      this.requestId = requestAnimationFrame(this.animate);
    },
    initScene() {
      this.scene = new THREE.Scene();
      const ambientLight = new THREE.AmbientLight(0xcccccc, 0.4);
      this.scene.add(ambientLight);
      // var axisHelper = new THREE.AxisHelper(15);
      // this.scene.add(axisHelper);
    },
    initCam() {
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      this.camera.position.z = 5;
      this.scene.add(this.camera);
    },
    initRenderer() {
      this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
      this.renderer.setSize(window.innerWidth , window.innerHeight );
      this.container = document.getElementById("container");
      this.container.appendChild(this.renderer.domElement);
    },
    autoRotate() {
      setInterval(() => {
        this.camera.rotation.y += (1 * Math.PI) / 180;
        // renderer.render( scene, camera );
        console.log("1");
      }, "1000");
    },
    loadModel() {
      let that = this ;
      const loader = new GLTFLoader();
      loader.load(
        "./fused.gltf",
        function (gltf) {
          that.scene.add(gltf.scene);
        },
        undefined,
        function (error) {
          console.error(error);
        }
      );
    },
    initControl() {
      this.controls = new OrbitControls(this.camera, this.CSSRender.domElement);
      this.controls.target = new THREE.Vector3(0, 0, 0);
      // 视角最小距离
      this.controls.minDistance = 12;
      // 视角最远距离
      this.controls.maxDistance = 100;
      // 最大角度
      this.controls.maxPolarAngle = Math.PI / 4;
    },
    create2DObject() {
      var div = document.createElement("div");

      div.style.width = "200px";
      div.style.height = "100px";
      div.style.background = "rgba(10,18,51,0.8)";
      div.style.color = "#fff";
      div.textContent = "hahaha";
      div.className = "label";

      const label = new CSS2DObject(div);
      return label;
    },
    initCSSRender() {
      this.CSSRender = new CSS2DRenderer();
      this.CSSRender.setSize(window.innerWidth, window.innerHeight );
      this.CSSRender.domElement.style.position = "absolute";
      this.CSSRender.domElement.style.top = 0;
      this.container.appendChild(this.CSSRender.domElement);
    },
  },
  mounted() {
    this.initScene();
    this.initCam();
    this.initRenderer();
    this.initCSSRender();
    this.initControl();
    this.loadModel();
    this.animate();
    // this.autoRotate();
  },
};
</script>
