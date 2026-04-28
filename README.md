# threejs-starter
Dieses Beispiel basiert auf der offiziellen Dokumentation von threejs – „Creating a
Scene“
https://threejs.org/manual/#en/creating-a-scene

**three.js** ist eine JavaScript-Bibliothek, mit der 3D-Grafik direkt im Browser erstellt
werden kann. Sie nutzt WebGL – die Szenen werden auf der Grafikkarte gerendert.  
Für Darstellungen von 3D Objekten benötigen wir 3 Objekte:
Eine **Szene** (*THREE.Scene*), eine **Kamera** (zB *THREE.PerspectiveCamera*) und einen
**Renderer** (*THREE.WebGLRenderer*)

```javascript
// Scene
const scene = new THREE.Scene();

// Camera
const camera = new THREE.PerspectiveCamera(
75, window.innerWidth / window.innerHeight, 0.1, 1000
);
camera.position.z = 5;

// Renderer
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```


## 01 Grundbausteine verstehen:
1. *Scene*  
Die Szene ist wie eine „leere Bühne“. Hier werden alle Dinge hinzugefügt, die angezeigt werden sollen – z. B. Objekte, Licht, etc.
2. *Camera*  
Die Kamera ist der Blick auf die Bühne. Sie bestimmt, was aus welchem Winkel und in welcher Entfernung gesehen wird.
3. *Renderer*  
Der Renderer rendert ( = „zeigt“) das Bild.
Er nimmt die Szene und Kamera und „zeichnet“ daraus ein sichtbares Bild auf den Bildschirm.

**Die Werte der Kamera:**  

Das erste Attribut ist das Sichtfeld (Field of View, FOV).
Das Sichtfeld gibt an, wie viel von der Szene man auf dem Bildschirm auf einmal sehen kann. Der Wert wird in Grad angegeben.

Das zweite Attribut ist das Seitenverhältnis (Aspect Ratio).
Du solltest fast immer die Breite durch die Höhe des Bildschirms oder Fensters teilen. Wenn du das nicht machst, passiert das Gleiche wie bei alten Filmen auf einem modernen Fernseher – das Bild wirkt gestaucht oder verzerrt.

Die nächsten beiden Attribute sind die nahe und ferne Clipping-Ebene. Das bedeutet: Objekte, die näher als „near“ oder weiter entfernt als „far“ sind, werden
nicht angezeigt.

**Echtzeit Rendering:**

Zum Schluss fügen wir das Renderer-Element zu unserem HTML-Dokument hinzu. Dabei handelt es sich um ein `<canvas>-Element`, das der Renderer nutzt, um uns die Szene anzuzeigen.

**Geometrie:**  

Für die Einfache Darstellung des Würfels verwenden wir eine Geometrie (*THREE.BoxGeometry*), ein Material (*THREE.MeshBasicMaterial*) und ein Mesh (*THREE.Mesh*).  

Am Ende wird der Würfel der Szene hinzugefügt.

```javascript
// Simple cube
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({ color: 0x00aaff });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);
```

**Die Animation:**

```javascript
// Animation loop
function animate() {
  requestAnimationFrame(animate);

  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;

  renderer.render(scene, camera);
}
animate();
```

- requestAnimationFrame(animate) sagt dem Browser:
„Ruf die Funktion animate beim nächsten Bild wieder auf.“
- Dadurch entsteht eine Endlosschleife, die aber vom Browser gesteuert wird – also
besonders flüssig und leistungsfreundlich.
- Der Browser passt die Animation an die Bildwiederholrate des Bildschirms an (z. B. 60
FPS).

In der Animations-Funktion können wir den Würfel in der Rotation immer wieder ein Stück weiterdrehen. Dadurch entsteht die Rotationsbewegung.

Am Ende wird der Render-Befehl aufgerufen, der das neue Kamerabild der Szene rendert.

> Experiment: 
> - Verändere die Richtung der Rotation (Lasse den Würfel in verschiedene Richtungen drehen)
> - Verändere die Rotationsgeschwindigkeit
> - Verändere die Farbe des Würfels

.
  
## 02 Licht
