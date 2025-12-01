# Archi AR - Augmented Reality untuk Presentasi Arsitektur

## 📋 Daftar Isi
- [Latar Belakang](#latar-belakang)
- [Fitur Utama](#fitur-utama)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Arsitektur Sistem](#arsitektur-sistem)
- [Cara Menggunakan](#cara-menggunakan)
- [Detail Teknis](#detail-teknis)
- [Persyaratan Sistem](#persyaratan-sistem)
- [Struktur Kode](#struktur-kode)

---

## Latar Belakang

### Masalah
Arsitek sering menghadapi tantangan dalam mempresentasikan desain mereka kepada klien:
- **Visualisasi 2D** (blueprint, denah) sulit dipahami oleh klien awam
- **Maquette fisik** mahal dan memakan waktu untuk dibuat
- **Render 3D** di layar tidak memberikan sense of scale yang akurat
- **Virtual Reality** memerlukan perangkat khusus yang mahal dan tidak praktis untuk presentasi di lapangan

### Solusi
**Archi AR** memanfaatkan teknologi Augmented Reality (AR) berbasis web untuk memungkinkan arsitek:
- Menempatkan model 3D arsitektur langsung di dunia nyata melalui smartphone/tablet
- Memberikan klien pengalaman immersive untuk "berjalan" di dalam desain (FPV Mode)
- Melakukan presentasi di mana saja tanpa perangkat tambahan
- Melihat skala sebenarnya dari desain dalam konteks lokasi nyata

### Nilai Tambah
- ✅ **Aksesibilitas**: Hanya perlu browser modern, tidak perlu aplikasi khusus
- ✅ **Portabilitas**: Berjalan di smartphone/tablet Android dan iOS
- ✅ **Interaktivitas**: Klien dapat berinteraksi langsung dengan model
- ✅ **Efisiensi Biaya**: Tidak perlu investasi hardware AR/VR mahal
- ✅ **Real-time Collaboration**: Presentasi dapat dilakukan langsung di lokasi proyek

---

## Fitur Utama

### 1. **Object Placement**
- Deteksi permukaan horizontal menggunakan WebXR Hit Testing
- Penempatan multiple model 3D di dunia nyata
- Visualisasi reticle untuk guided placement

### 2. **Gesture Controls**
- **1 Jari (Swipe)**: Rotasi objek pada sumbu Y
- **2 Jari (Pinch)**: Scale objek (0.1x - 5.0x)
- **Tap**: Seleksi/deseleksi objek

### 3. **Model Library**
- Gallery model 3D dengan preview icon
- 6+ model arsitektur siap pakai:
  - Tower House Design
  - Kitchen Interior
  - 3 Bedroom House
  - Apartment Floor Plan
  - Room Layout
  - Astronaut (contoh)
- Dynamic loading untuk efisiensi memori

### 4. **FPV (First-Person View) Mode**
- **Scaling Otomatis**: Model diperbesar 20x untuk skala 1:1
- **Virtual Walking**: Joystick untuk navigasi dalam model
- **Collision Detection**: Mencegah user melewati dinding/objek
- **Vertical Controls**: Tombol Up/Down untuk pergerakan lantai
- **Centering Otomatis**: Viewer ditempatkan di pusat model saat masuk FPV

### 5. **Object Management**
- Selection system dengan highlight box
- Delete individual objects
- Reset all objects
- Persistent object tracking

### 6. **UI/UX Modern**
- Glass-morphism design
- Smooth animations & transitions
- Touch-optimized controls
- Real-time toast notifications
- DOM Overlay untuk UI yang responsive

---

## 🛠 Teknologi yang Digunakan

### Core Technologies
| Teknologi | Versi | Fungsi |
|-----------|-------|--------|
| **WebXR Device API** | Latest | Framework AR berbasis web standar W3C |
| **Three.js** | r126 | 3D rendering engine |
| **GLTFLoader** | r126 | Loading model 3D format .glb/.gltf |
| **Nipplejs** | 0.10.1 | Virtual joystick untuk FPV controls |

### Web Standards
- **HTML5**: Struktur aplikasi
- **CSS3**: Styling dengan custom properties, backdrop-filter, animations
- **ES6+ JavaScript**: OOP architecture, async/await, Promises

### WebXR Features
- `immersive-ar`: Mode AR immersive
- `hit-test`: Deteksi permukaan real-world
- `dom-overlay`: UI overlay di atas AR view

---

## Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────┐
│                    User Interface Layer                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │ Start Screen │  │ Standard UI  │  │   FPV UI     │   │
│  │   - Init     │  │  - Library   │  │  - Joystick  │   │
│  │   - Status   │  │  - Actions   │  │  - Elevation │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                Application Logic Layer                  │
│  ┌────────────────────────────────────────────────────┐ │
│  │         ARObjectPlacement Class (MVC Pattern)      │ │
│  │                                                    │ │
│  │  • State Management (fpvMode, selectedObject)      │ │
│  │  • Event Handlers (touch, select, XR events)       │ │
│  │  • Business Logic (collision, placement)           │ │
│  │  • UI Controllers (toast, drawer, transitions)     │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                  Rendering & XR Layer                   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │   Three.js   │  │  WebXR API   │  │ Hit Testing  │   │
│  │   Renderer   │  │   Session    │  │    Source    │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                   Device Hardware Layer                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │    Camera    │  │  Accelero-   │  │   Display    │   │
│  │              │  │    meter     │  │              │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## 📱 Cara Menggunakan

### Persiapan
1. Buka aplikasi di browser yang support WebXR (Chrome Android/iOS)
2. Pastikan izin kamera sudah diberikan
3. Gunakan di area dengan pencahayaan yang cukup

### Workflow Standar

#### 1. Memulai AR Session
```
Tap "Mulai AR" → Arahkan ke permukaan horizontal → 
Tunggu reticle muncul → Siap menempatkan objek
```

#### 2. Memilih & Menempatkan Model
```
Tap "📂 Library 3D" → Pilih model → 
Tap area dengan reticle → Objek ditempatkan
```

#### 3. Memanipulasi Objek
```
Tap objek untuk select →
• 1 Jari Swipe: Rotasi
• 2 Jari Pinch: Scale
• Tap "Hapus": Delete objek
```

#### 4. Masuk FPV Mode
```
Select objek → Tap "🚶 FPV" →
• Joystick: Bergerak horizontal
• ▲/▼ Buttons: Naik/turun lantai
• Collision detection aktif
```

#### 5. Keluar & Reset
```
Dalam FPV: Tap "✕" → Kembali ke mode normal
Mode Normal: Tap "↺" → Reset semua objek
```

---

## 🔧 Detail Teknis

### 1. WebXR Session Initialization
```javascript
const session = await navigator.xr.requestSession("immersive-ar", {
    requiredFeatures: ['hit-test'],
    optionalFeatures: ['dom-overlay'],
    domOverlay: { root: document.getElementById('ar-root') }
});
```

**Key Points:**
- `hit-test`: Wajib untuk deteksi permukaan
- `dom-overlay`: Memungkinkan UI HTML di atas AR view
- `#ar-root`: Container DOM yang di-overlay

### 2. Hit Testing Implementation
```javascript
handleHitTest(frame) {
    const hitTestResults = frame.getHitTestResults(this.hitTestSource);
    if (hitTestResults.length > 0) {
        const hitPose = hitTestResults[0].getPose(this.referenceSpace);
        this.reticle.position.copy(hitPose.transform.position);
        this.reticle.visible = true;
    }
}
```

**Proses:**
1. Frame XR memberikan hit test results per frame
2. Mengambil pose dari hit pertama (paling dekat)
3. Update posisi reticle sesuai permukaan terdeteksi

### 3. FPV Mode Transformasi

#### Scaling & Positioning
```javascript
const targetScale = this.fpvOriginalScale.x * 20; // 20x multiplier
this.fpvObject.scale.set(targetScale, targetScale, targetScale);

// Centering calculation
const box = new THREE.Box3().setFromObject(this.fpvObject);
const center = box.getCenter(new THREE.Vector3());
const baseY = box.min.y;

this.fpvObject.position.y = -this.playerHeight - baseY;
this.fpvObject.position.x = -center.x;
this.fpvObject.position.z = -center.z;
```

**Logika:**
- Objek diperbesar 20x untuk skala manusia 1:1
- Viewer (Y=0) ditempatkan `playerHeight` (1.8m) di atas base objek
- Objek di-center horizontal agar viewer berada di tengah

### 4. Collision Detection System

```javascript
checkFPVCollisions(movementVector) {
    // Horizontal collision (walls)
    const rays = [
        horizontalDirection,
        horizontalDirection.clone().applyAxisAngle(Y_AXIS, PI/4),  // 45° left
        horizontalDirection.clone().applyAxisAngle(Y_AXIS, -PI/4)  // 45° right
    ];
    
    rays.forEach(ray => {
        raycaster.set(playerPos, ray);
        raycaster.far = playerRadius + movement.length();
        const hits = raycaster.intersectObjects(collisionObjects);
        
        if (hits.length > 0 && hits[0].distance < playerRadius) {
            // Slide along wall normal
            movement = movement.projectOnPlane(hit.normal);
        }
    });
    
    // Vertical collision (ceiling/floor)
    // ... similar logic for up/down rays
}
```

**Features:**
- **Multi-ray casting**: 3 rays (front, front-left, front-right) untuk deteksi lebih robust
- **Player capsule**: Radius 0.5m, height 1.8m
- **Sliding**: User "meluncur" di sepanjang dinding, bukan stuck
- **Vertical checks**: Mencegah tembus ceiling/floor

### 5. Touch Gesture Recognition

```javascript
onTouchStart(e) {
    if (e.touches.length === 2) {
        // Pinch gesture initialization
        this.startDist = Math.hypot(dx, dy);
        this.startScale = object.scale.x;
    } else {
        // Rotation gesture initialization
        this.touchStartX = e.touches[0].pageX;
        this.initialRotation = object.rotation.y;
    }
}

onTouchMove(e) {
    if (e.touches.length === 2) {
        // Pinch-to-scale
        const scaleFactor = currentDist / startDist;
        const newScale = clamp(startScale * scaleFactor, 0.1, 5.0);
    } else {
        // Swipe-to-rotate
        const dx = e.touches[0].pageX - this.touchStartX;
        object.rotation.y = initialRotation + (dx * sensitivity);
    }
}
```

**Anti-conflict Measures:**
- `isUIInteraction` flag mencegah gesture saat interact dengan UI
- `isDragging` flag mencegah accidental placement
- Debouncing 200ms setelah UI interaction

### 6. Model Loading Pipeline

```javascript
async loadSpecificModel(loader, modelUrl) {
    const gltf = await this.loadGLTF(loader, modelUrl);
    this.arObject = gltf.scene;
    
    // Optimization
    this.arObject.traverse((child) => {
        if (child.isMesh) {
            child.castShadow = true;
            child.receiveShadow = true;
            if (child.material?.map) {
                child.material.map.encoding = THREE.sRGBEncoding;
            }
        }
    });
}
```

**Optimizations:**
- Shadow casting enabled untuk realism
- sRGB encoding untuk color accuracy
- Lazy loading (hanya model aktif yang di-load)

---

## 💻 Persyaratan Sistem

### Browser Support
| Platform | Browser | Minimum Version |
|----------|---------|-----------------|
| Android | Chrome | v79+ |
| Android | Edge | v79+ |
| iOS | Safari | v15.4+ |
| iOS | Chrome* | v108+ |

*Chrome iOS menggunakan WebKit engine, support terbatas

### Device Requirements
- **Kamera**: Rear camera dengan autofocus
- **Sensors**: Accelerometer, Gyroscope, Magnetometer
- **OS**: Android 7.0+ atau iOS 15.4+
- **RAM**: Minimum 2GB (recommended 4GB+)
- **Connection**: Tidak perlu internet setelah initial load

### Permission Requirements
```javascript
// Browser akan auto-request permission untuk:
- Camera access
- Motion sensors (DeviceOrientation)
```

---


## 📂 Struktur Kode

### Main Class: `ARObjectPlacement`

```javascript
class ARObjectPlacement {
    // ===== PROPERTIES =====
    // WebXR Core
    session, referenceSpace, viewerSpace, hitTestSource
    
    // Three.js Core
    scene, camera, renderer, raycaster
    
    // AR Objects
    reticle, arObject, placedObjects[], selectedObject
    
    // FPV State
    fpvMode, fpvObject, fpvOriginal{Scale|Position|Rotation}
    moveVec, verticalSpeed, joystick
    
    // Collision
    playerHeight, playerRadius, collisionRaycaster
    
    // UI State
    isUIInteraction, isDragging, availableModels[]
    
    // ===== LIFECYCLE METHODS =====
    constructor()       // Initialize properties
    init()              // Async setup
    checkWebXRSupport() // Validate browser capabilities
    loadModels()        // Load default assets
    startAR()           // Initialize XR session
    onXRFrame()         // Per-frame render loop
    onSessionEnded()    // Cleanup on exit
    
    // ===== CORE AR FEATURES =====
    handleHitTest()     // Surface detection
    onSelect()          // Handle XR select events
    placeObject()       // Instantiate model at reticle
    selectObject()      // Object selection logic
    deselectObject()    // Clear selection
    deleteSelectedObject() // Remove from scene
    
    // ===== FPV MODE =====
    enterFPVMode()      // Scale up & center object
    exitFPVMode()       // Restore original transform
    handleFPVMovement() // Process joystick input
    checkFPVCollisions()// Raycast collision detection
    
    // ===== GESTURES =====
    onTouchStart()      // Initialize gesture tracking
    onTouchMove()       // Process rotation/scaling
    onTouchEnd()        // Finalize gesture
    
    // ===== UI HELPERS =====
    renderLibrary()     // Populate model gallery
    showToast()         // Display notification
    updateHighlightBox()// Update selection visual
}
```

### Key Design Patterns

#### 1. **State Machine Pattern**
```javascript
// UI states
Normal Mode → Object Selected → FPV Mode
     ↑              ↓                ↓
     └──────────────┴────────────────┘
```

#### 2. **Observer Pattern**
```javascript
// Event listeners propagate state changes
session.addEventListener('select', onSelect)
window.addEventListener('touchstart', onTouchStart)
joystick.on('move', updateMoveVector)
```

#### 3. **Factory Pattern**
```javascript
// Model instantiation
placeObject() {
    const clone = this.arObject.clone(); // Factory
    clone.userData.objectId = this.objectIndex++;
    this.scene.add(clone);
}
```

---

## 🔮 Pengembangan Lebih Lanjut

### Fitur Roadmap

#### v1.1 - Material Editor
- [ ] Real-time material swapping (kayu, beton, kaca)
- [ ] Color picker untuk walls/floors
- [ ] Texture library integration

#### v1.2 - Measurement Tools
- [ ] Ruler tool untuk ukur jarak
- [ ] Area calculator untuk lantai
- [ ] Height indicator untuk ceiling

#### v1.3 - Collaboration
- [ ] Multi-user AR session via WebRTC
- [ ] Cloud save/load projects
- [ ] Screenshot & video capture

#### v1.4 - Advanced Physics
- [ ] Realistic gravity simulation
- [ ] Furniture placement snapping
- [ ] Auto-layout suggestions

---


**Built with ❤️ for you all**