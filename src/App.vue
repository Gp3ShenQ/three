<template>
  <div>
    <canvas id="three-canvas"></canvas>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import * as THREE from 'three';

const initThree = () => {
  // 創建場景
  const scene = new THREE.Scene();

  // 創建相機
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.set(0, 0, 5);

  // 創建渲染器
  const renderer = new THREE.WebGLRenderer({
    canvas: document.getElementById('three-canvas'),
  });
  renderer.setSize(window.innerWidth, window.innerHeight);

  // 創建立方體
  const boxGeometry = new THREE.BoxGeometry(1, 1, 1, 2, 2, 2);
  const boxMaterial = new THREE.MeshBasicMaterial({ color: 0x00ff00, wireframe: true });
  const cube = new THREE.Mesh(boxGeometry, boxMaterial);
  cube.position.x = -2; // 將立方體移到左邊
  scene.add(cube);

  // 創建球體幾何體
  const sphereGeometry = new THREE.SphereGeometry(1, 1, 1);
  const sphereMaterial = new THREE.MeshBasicMaterial({ color: 0x0077ff });
  const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
  sphere.position.x = 2; // 將球體移到右邊
  scene.add(sphere);

  // 創建 3D 三角錐幾何體
  const coneGeometry = new THREE.ConeGeometry(1, 1, 100); // 半徑、高度、圓周分段數
  const coneMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000, wireframe: true });
  const cone = new THREE.Mesh(coneGeometry, coneMaterial);
  cone.position.y = 2; // 將球體移到右邊
  scene.add(cone);

  // 創建 3D 圓柱體幾何體
  const cylinderGeometry = new THREE.CylinderGeometry(1, 1, 2, 32); // 上半徑、下半徑、高度、圓周分段數
  const cylinderMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00, wireframe: true });
  const cylinder = new THREE.Mesh(cylinderGeometry, cylinderMaterial);
  cylinder.position.y = -2; // 將球體移到右邊
  scene.add(cylinder);

  // 设置旋转中心点
  const target = new THREE.Vector3(0, 0, 0);

  // 渲染循环
  const clock = new THREE.Clock();
  const animate = () => {
    requestAnimationFrame(animate);

    // 這邊是選轉相機
    const elapsed = clock.getElapsedTime();
    const radius = 9; // 相机围绕旋转的半径
    const height = 3; // 相机在y轴上的高度偏移
    camera.position.x = radius * Math.cos(elapsed);
    camera.position.y = height; // 增加相机的高度
    camera.position.z = radius * Math.sin(elapsed);
    camera.lookAt(target);

    // 旋轉立方體
    cube.rotation.x += 0.005;
    cube.rotation.y += 0.005;
    // 旋轉球體
    sphere.rotation.x += 0.005;
    sphere.rotation.y += 0.005;
    // 旋轉三角錐
    cone.rotation.x += 0.01;
    cone.rotation.y += 0.01;
    // 旋轉圓柱體
    cylinder.rotation.x += 0.01;
    cylinder.rotation.y += 0.01;

    renderer.render(scene, camera);
  };

  animate();

  // 處理窗口大小變更
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });
};

onMounted(() => {
  initThree();
});
</script>

<style scoped lang="scss">
body,
html {
  margin: 0;
  padding: 0;
}

canvas {
  display: block;
  width: 100vw;
  height: 100vh;
}
</style>
