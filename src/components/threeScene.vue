<template>
    <div id="renderElement">
        <div id="css"></div>
        <div id="webgl"></div>
    </div>
</template>

<script>
// THREE Imports
import * as THREE from 'three';
import { CSS3DRenderer, CSS3DObject} from 'three/examples/jsm/renderers/CSS3DRenderer';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

// Components
import Vue from 'vue'
import htmlPanel from './htmlPanel.vue';

export default {
    name: 'ThreeScene',
    components: {},
    data() {
        return {
            camera: null,
            scene: null,
            scene2: null,
            renderer: null,
            renderer2: null,
            controls: null
        }
    },
    methods: {
        initThree(){

            this.camera = new THREE.PerspectiveCamera(
                45, window.innerWidth / window.innerHeight, 1, 2000
            );
            this.camera.position.set( 0, 0, 500 );

            this.scene = new THREE.Scene();
            this.scene2 = new THREE.Scene();
            
            var material = new THREE.MeshPhongMaterial({
                color: 0x156289,
                emissive: 0x000000,
                specular: 0x111111,
                side: THREE.DoubleSide,
                flatShading: false,
                shininess: 30,
                
            })
            var geometry = new THREE.SphereGeometry( 70, 32, 32 );
            var sphere = new THREE.Mesh( geometry, material );
            sphere.position.z = 100;
            sphere.castShadow = true;
            sphere.receiveShadow = false;
            this.scene.add( sphere );
        
            // light
            
            var ambientLight = new THREE.AmbientLight( 0x000000 );
            this.scene.add( ambientLight );

            var lights = new THREE.PointLight( 0xffffff, 1, 0 );
            lights.castShadow = true;
            lights.position.z = 300;
            lights.shadow.mapSize.width = 256;  // default
            lights.shadow.mapSize.height = 256; // default
            lights.shadow.camera.near = 1;       // default
            lights.shadow.camera.far = 2000;      // default
            this.scene.add( lights );
            //

            this.renderer2 = new CSS3DRenderer();
            this.renderer2.setSize( window.innerWidth, window.innerHeight );
            this.renderer2.domElement.style.position = 'absolute';
            this.renderer2.domElement.style.top = 0;
            document.getElementById('css').appendChild( this.renderer2.domElement );

            this.renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
            this.renderer.setClearColor( 0x000000, 0 );
            this.renderer.setPixelRatio( window.devicePixelRatio );
            this.renderer.setSize( window.innerWidth, window.innerHeight );
            this.renderer.shadowMap.enabled = true;
            this.renderer.shadowMap.type = THREE.PCFSoftShadowMap; // default THREE.PCFShadowMap
            document.getElementById('webgl').appendChild( this.renderer.domElement );

            let appEl = document.getElementById("app");

            this.controls = new OrbitControls( this.camera, appEl);

            const div = document.createElement('div');
            const componentInstance = new Vue(Object.assign({}, htmlPanel)).$mount(div);
            const panel = componentInstance.$el;

            let domObject = new CSS3DObject( panel );
            domObject.position.set(1,0,0);
            domObject.rotation.set(0,0,0);
            this.scene2.add( domObject );

            var material2 = new THREE.MeshPhongMaterial({
                    opacity	: 0.2,
                    color	: new THREE.Color('black'),
                    blending: THREE.NoBlending,
                    side	: THREE.DoubleSide,
                });
            var geometry2 = new THREE.PlaneGeometry( 100, 100 );
            var mesh = new THREE.Mesh( geometry2, material2 );
            mesh.position.copy( domObject.position );
            mesh.rotation.copy( domObject.rotation );
            mesh.castShadow = false;
            mesh.receiveShadow = true;
            this.scene.add( mesh );
        },
        animate() {
            this.scene.updateMatrixWorld()

            this.controls.update();

            this.renderer.render( this.scene, this.camera );
            this.renderer2.render( this.scene2, this.camera );

            requestAnimationFrame( this.animate );
        },
    },
    
    mounted(){
        this.initThree();
        this.animate();
    }

}
</script>


<style scoped>
#css, #webgl {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0; 
    left: 0;
}
</style>