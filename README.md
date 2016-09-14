# BVHImporter

Note: if you're looking for a Three.js BVH Loader, there is a modified version of this one included the official Three.js examples loader collection: [http://threejs.org/examples/?q=loader#webgl_loader_bvh](http://threejs.org/examples/?q=loader#webgl_loader_bvh)

JavaScript parser for BVH files and converter to Three.js Bones.

`BVHImport.readBvh(lines)` parses a bvh file and creates basic tech-agnostic representation of the bvh nodes including motion data.

`BVHImport.toTHREE(source)` converts the internal reperesentation to a THREE.Skeleton and a single THREE.AnimationClip which can be passed straight to a THREE.AnimationMixer


## Usage
(see [http://herzig.github.io/BVHImporter/](http://herzig.github.io/BVHImporter/) for the full working example.
Parsing a BVH file and create a Three.js skeleton & keyframe animation:
```
// import BVH file (from string array).
var root = BVHImport.readBvh(lines);

// animation contains a THREE.Skeleton and the THREE.AnimationClip
var animation = BVHImport.toTHREE(root);

// create a minimal empty geometry and skinned mesh to hold the bones
var geometry = new THREE.Geometry(); 
var material = new THREE.MeshPhongMaterial({ skinning: true, });
var mesh = new THREE.SkinnedMesh(geometry, material);

// bind skeleton
mesh.add(animation.skeleton.bones[0]);
mesh.bind(animation.skeleton);

skeletonHelper = new THREE.SkeletonHelper(mesh);
skeletonHelper.material.linewidth = 5;

scene.add(skeletonHelper);
scene.add(mesh);

mixer = new THREE.AnimationMixer(mesh);
mixer.clipAction(animation.clip).setEffectiveWeight(1.0).play(
````
