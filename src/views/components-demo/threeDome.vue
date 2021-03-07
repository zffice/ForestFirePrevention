<template>
  <div></div>
</template>
<script>
import * as THREE from 'three'
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { TextureLoader } from 'three';
export default {
  data () {
    return {
      scene: null,
      camera: null,
      mesh: null
    }
  },
  mounted () {
    this.init()
    this.render()
    var controls = new OrbitControls(this.camera, this.renderer.domElement)
  },
  methods: {
    init () {
      this.scene = new THREE.Scene();
      //创建场景对象Scene
      // 创建一个草地地面
      //矩形平面
      var gem = new THREE.PlaneGeometry(10000, 10000)
      // 加载草地纹理贴图
      var texture = new THREE.TextureLoader().load('src/assets/images/grass.jpg')
      // 设置纹理的重复模式
      texture.wrapS = THREE.RepeatWrapping;
      texture.wrapT = THREE.RepeatWrapping;
      // uv两个方向纹理重复数量
      texture.repeat.set(10, 10)

      var material = new THREE.MeshLambertMaterial({
        color: 0x777700,
        map: texture
      })
      //网格模型对象Mesh
      var mesh = new THREE.Mesh(gem, material)
      //网格模型添加到场景中
      this.scene.add(mesh)
      mesh.rotateX(-Math.PI / 2)

      // 树的模型
      var textureTree = new THREE.TextureLoader().load('../../assets/images/tree.png')
      // 批量创建表示一个树的精灵模型
      for (let i = 0; i < 100; i++) {
        var spriteMaterial = new THREE.SpriteMaterial({
          map: textureTree,//设置精灵纹理贴图
        });
        // 创建精灵模型对象
        var sprite = new THREE.Sprite(spriteMaterial);
        this.scene.add(sprite);
        // 控制精灵大小,
        sprite.scale.set(100, 100, 1); // 只需要设置x、y两个分量就可以
        var k1 = Math.random() - 0.5;
        var k2 = Math.random() - 0.5;
        // 设置精灵模型位置，在xoz平面上随机分布
        sprite.position.set(1000 * k1, 50, 1000 * k2)
      }

      var point = new THREE.PointLight(0xffffff);
      point.position.set(400, 200, 300);
      this.scene.add(point)
      var axesHelper = new THREE.AxesHelper(5000)
      this.scene.add(axesHelper)
      var ambient = new THREE.AmbientLight(0x888888);
      this.scene.add(ambient);

      var width = window.innerWidth; //窗口宽度
      var height = window.innerHeight; //窗口高度
      var k = width / height; //窗口宽高比

      //创建相机对象
      this.camera = new THREE.PerspectiveCamera(60, k, 1, 10000);
      this.camera.position.set(200, 5, 200); //设置相机位置
      this.camera.lookAt(this.scene.position); //设置相机方向(指向的场景对象)

      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(width, height);//设置渲染区域尺寸
      this.renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
      document.body.appendChild(this.renderer.domElement); //body元素中插入canvas对象

      this.renderer.render(this.scene, this.camera)
    },
    render () {
      this.renderer.render(this.scene, this.camera)
      // this.mesh.rotateY(0.1)
      requestAnimationFrame(this.render)
    }
  }
}
</script>