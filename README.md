# threejs_demo

## rain 下雨
threejs两种方式实现下雨效果：第一种方式几乎无性能问题，第二种方式在复杂场景下会有性能问题。

## 1、THREE.InstancedMesh + matrix

### 核心代码
```javascript
// 创建一个简单的几何体来表示雨滴（用平面几何体代替雨滴模型）
const rainGeometry = new THREE.PlaneGeometry(20, 30);  // 可根据需要调整雨滴的大小
const rainMaterial = new THREE.MeshBasicMaterial({
	map: rainTexture,
	transparent: true,
	opacity: 0.8,
	depthWrite: false,  // 防止深度冲突
	side: THREE.DoubleSide  // 设置正反面都可见
});
const rainCount = 5000;
// 使用 InstancedMesh 优化性能
const rainInstancedMesh = new THREE.InstancedMesh(rainGeometry, rainMaterial, rainCount);
```

### 功能特点

- **InstancedMesh 优化性能**: 使用 `InstancedMesh` 来高效渲染大量雨滴，避免了逐个渲染每个独立网格带来的性能开销。
- **贴图应用**: 为雨滴应用透明的贴图（`rain4.png`），实现逼真的视觉效果。
- **随机分布**: 雨滴随机分布在定义的 3D 空间范围内，模拟更自然的降雨效果。
- **Orbit Controls 控制**: 使用 `OrbitControls` 来控制相机视角，支持平滑的相机移动、缩放。
- **HDR 环境贴图**: 可选加载 `.hdr` 环境贴图，用于创建动态光照和反射效果。

### 关键功能

- **InstancedMesh 实现雨滴**:
    - 雨滴使用 `InstancedMesh` 来表示，每个雨滴是一个带贴图的平面几何体。
    - 每一帧动态更新每个雨滴的位置，实现其下落效果。

- **动画效果**:
    - 每个雨滴的 `y` 轴位置会在每帧更新，模拟雨滴不断下落的效果。当雨滴的位置超过一定范围后，会重置其位置，形成连续的下落效果。

- **随机位置分布**:
    - 雨滴在场景中随机生成，分布范围内的每个雨滴都有一个随机的位置，模拟自然的降雨效果。

- **Orbit Controls**:
    - 使用 `OrbitControls` 使用户能够交互式地旋转、缩放和移动相机视角，探索场景。

### 如何自定义

- **雨滴贴图**:
    - 可以替换 `/rain4.png` 为您自定义的雨滴贴图，以获得不同的视觉效果。

- **雨滴数量**:
    - 可以调整 `rainCount` 变量来增加或减少场景中的雨滴数量。

- **雨滴下落速度**:
    - 通过修改 `dummy.position.y -= 20.0;` 这行代码，您可以调整雨滴下落的速度。

 ![image](https://github.com/leiyun1993/threejs_demo/raw/main/screenshot/image1.png)

 ## 2、THREE.Points
### 核心代码

```javascript
const rainGeometry = new THREE.BufferGeometry();
const rainPositions = new Float32Array(rainCount * 3);

for (let i = 0; i < rainCount; i++) {
	rainPositions[i * 3] = getRandomX();  // X轴位置
	rainPositions[i * 3 + 1] = getRandomY();// Y轴位置
	rainPositions[i * 3 + 2] = getRandomZ();  // Z轴位置
}

rainGeometry.setAttribute('position', new THREE.BufferAttribute(rainPositions, 3));

const rainMaterial = new THREE.PointsMaterial({
	size: 20, // 雨滴大小
	map: rainTexture,
	transparent: true,
	opacity: 0.8
});

const rain = new THREE.Points(rainGeometry, rainMaterial);
```

 ### 自定义

- **雨滴贴图**:
    - 可以替换 `/rain4.png` 为您自定义的雨滴贴图，以获得不同的视觉效果。

- **雨滴数量**:
    - 可以调整 `rainCount` 变量来增加或减少场景中的雨滴数量。

- **雨滴下落速度**:
    - 通过修改 `positions[i * 3 + 1] -= 20.0;` 这行代码，您可以调整雨滴下落的速度。

 ![image](https://github.com/leiyun1993/threejs_demo/raw/main/screenshot/image2.png)

 ### 启动方式

 ```
 npm install

 npm start

 http://127.0.0.1:8080/rain.html	//方式1

 http://127.0.0.1:8080/rain2.html //方式2
 ```