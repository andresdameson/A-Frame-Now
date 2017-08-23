# WebVR para todos: Introducción a la realidad virtual con Aframe.


La web dejó de ser un espacio plano donde los contenidos solo hacen scroll para dar lugar a una nueva forma de representar la información. Desde hace un tiempo diferentes tecnologías nos permiten crear espacios funcionales y darles movimiento e immersion.
A-frame es una tecnología opensource que permite integrar a nuestra web diferentes aspectos de realidad virtual de manera fácil y efectiva utilizando los conocimientos que ya tenemos sobre la web actual.

Durante este workshop vamos a aprender:<br/>
* Nociones básicas de espacialidad y web inmersiva. <br/>
* Cómo utilizar conocimientos básicos sobre JS y HTML para desarrollar con A-FRAME. <br/>

Además, vamos a construir tu primera web inmersiva.

## Presentación

Podes ver la presentación en el siguiente [link](linkpresentacion.com)

## Requisitos
1. Computadora con navegador Firefox o Chrome.
2. Algún servidor web o un embebido como http-server.
3. Tu editor de texto favorito.

## Setup

### Instalación

Usando `npm`

```bash
git clone git@github.com:repo/workshop-project.git
cd folder
npm install
npm start
```

Esto va a levantar un servidor y va a abrir una ventana del navegador con la pagina inicial.

### Contenido

1. [A-Frame y WebVR](#aframe-webvr)
2. [Empezando](#empezando)
3. [Creando nuestra escena](#nuestra-escena)
4. [Agregando interaccion](#interaccion)

## Donde ir a partir de aqui
Para conocer mas sobre este framework no dejes de visitar el sitio web de [A-Frame](https://www.aframe.io)



## A-Frame y WebVR {#aframe-webvr}

Presentacion de A-Frame y WebVR






## Creando nuestra escena {#nuestra-escena}

Vamos a crear nuestra primera experencia inmersiva con una imagen de 360.
En el documento que veniamos trabajando, borramos la caja y el cielo del HTML. Cambiamos la propiedad de radio de la esfera para hacerla gigante, y la movemos de manera que la camara quede dentro.

```
<a-scene>
    <a-sphere position="0 2 0" radius="5" color="#EF2D5E"></a-sphere>
    <a-camera>
    <a-cursor id="cursor"></a-cursor>
  </a-camera>
</a-scene>
```

Pero a pesar dentro de la esfera, podemos ver a traves de la misma, en lugar de ver su interior. Eso es porque nos falto agregar el atributo side "double", que refleja la misma textura que tiene una primitiva tanto dentro como fuera de la misma.

```
<a-scene>
    <a-sphere position="0 2 0" radius="5" material="src: pen/imagen/paisaje-360.jpg;" side="double"></a-sphere>
    <a-camera>
    <a-cursor id="cursor"></a-cursor>
  </a-camera>
</a-scene>
```

El atributo material nos permite colocar propiedades relacionadas a la textura que le aplicamos a nuestros elementos. En el ejemplo agregamos la imagen de un paisaje 360.


## Agregando interaccion {#interaccion}

Nuestro objetivo aqui es agregar un par de botones a la experiencia. haciendo click en uno u otro cambiaremos la imagen de la esfera en la que estamos inmersos.

Comenzamos agregando un plano con algunos atributos. El plano funcionara como boton.

```
<a-scene>
    <a-sphere position="0 2 0" radius="5" material="src: pen/imagen/paisaje-360.jpg;" side="double"></a-sphere>
    <a-plane
        position="0 2 -3"
        rotation="0 0 0"
        width="1"
        height="1"
        material="src: pen/imagen/boton-1.jpg";
    ></a-plane>
    <a-camera>
    <a-cursor id="cursor"></a-cursor>
  </a-camera>
</a-scene>
```

Ahora vamos a darle un poco de movimiento a nuestro boton. A-Frame maneja las animaciones de la misma manera que las primitivas, por medio de elementos con atributos. Para agregar animacion a una primitiva, la colocamos dentro de esta. EN nuestro ejemplo, agregamos dos animaciones, una para animar la posicion y otra la rotacion. En cada animacion podemos setear la propiedad a animar, su duracion, direccion, repeticion, entre otras.

```
<a-scene>
    <a-sphere position="0 2 0" radius="5" material="src: pen/imagen/paisaje-360.jpg;" side="double"></a-sphere>
    <a-plane position="0 2 -3" rotation="0 0 0" width="1" height="1" material="src: pen/imagen/boton-1.jpg"; side="double">
        <a-animation
            attribute="position"
            dur="1000"
            to="0 2.2 -3"
            direction="alternate-reverse"
            repeat="indefinite"
        ></a-animation>
        <a-animation
            attribute="rotation"
            dur="1000"
            to="0 360 0"
            direction="alternate-reverse"
            repeat="indefinite"
        ></a-animation>
    </a-plane>
    <a-camera wasd-controls-enabled="false">
    <a-cursor id="cursor"></a-cursor>
  </a-camera>
</a-scene>
```
En la camara agregamos el atributo wasd-controls-enabled en false. Con esto restringimos la experiencia a permanecer siempre en el mismo lugar, bloqueando las funciones de desplazamiento con el teclado en las teclas W,A,S y D.

La animacion de rotacion en el plano no vamos a necesitarla por ahora, asi es que la removemos, y agregamos un script para cambiar la imagen de la esfera cuando el usuario hace click con el cursor apuntando al plano.

```
<a-scene>
    <a-sphere position="0 2 0" radius="5" material="src: pen/imagen/paisaje-360.jpg;" side="double"></a-sphere>
    <a-plane position="0 2 -3" rotation="0 0 0" width="1" height="1" material="src: pen/imagen/boton-1.jpg"; side="double">
        <a-animation attribute="position" dur="1000" to="0 2.2 -3" direction="alternate-reverse" repeat="indefinite" ></a-animation>
    </a-plane>
    <a-camera>
    <a-cursor id="cursor"></a-cursor>
  </a-camera>
</a-scene>
<script>
    document.querySelector('a-plane').addEventListener('click', function (evt) {
        document.querySelector('a-sphere').setAttribute("material", "src: pen/imagen/ciudad-360.jpg;");
    });
</script>
```

Agregamos otro plano. Este funcionara como segundo boton para devolvernos a la imagen inicial. La idea aqui es que tengamos dos planos en la escena, uno visible y el otro oculto. Cuando cliqueamos el visible, la imagen de la esfera cambia. EL plano visible se mueve a una posicion mas lejana saliendo de nuestro campo de vision. El plano oculto se hace visible.


```
<a-scene>
    <a-sphere position="0 2 0" radius="5" material="src: pen/imagen/paisaje-360.jpg;" side="double"></a-sphere>

    <a-plane id="boton-1" position="0 2 -3" rotation="0 0 0" width="1" height="1" material="src: pen/imagen/boton-1.jpg"; side="double"></a-plane>

    <a-plane id="boton-2" position="2 100 -3" rotation="0 0 0" width="1" height="1" material="src: pen/imagen/boton-2.jpg"; side="double"></a-plane>

    <a-camera wasd-controls-enabled="false">
    <a-cursor id="cursor"></a-cursor>
  </a-camera>

</a-scene>
<script>
    document.querySelector('#boton-1').addEventListener('click', function() {
        this.setAttribute("position", "0 100 -3");
        document.querySelector('#boton-2').setAttribute("position", "2 2 -3");
        document.querySelector('a-sphere').setAttribute("material", "src: pen/imagen/ciudad-360.jpg;");
    });
    document.querySelector('#boton-2').addEventListener('click', function() {
        this.setAttribute("position", "2 100 -3");
        document.querySelector('a-sphere').setAttribute("material", "src: pen/imagen/cubos-360.jpg;");
    });
</script>
```

Aunque seria mas comodo si ambos estuviesen en el mismo lugar al clickearse. Lo que haremos entonces sera modificar la posicion inicial de ambos planos para intercalarlos.

```
<a-scene>

    <a-sphere position="0 2 0" radius="5" material="src: pen/imagen/paisaje-360.jpg;" side="double"></a-sphere>

    <a-plane id="boton-1" position="0 2 -3" rotation="0 0 0" width="1" height="1" material="src: pen/imagen/boton-1.jpg"; side="double"></a-plane>

    <a-plane id="boton-2" position="0 100 -3" rotation="0 0 0" width="1" height="1" material="src: pen/imagen/boton-2.jpg"; side="double"></a-plane>

    <a-camera wasd-controls-enabled="false">
        <a-cursor id="cursor"></a-cursor>
    </a-camera>

</a-scene>
<script>
    document.querySelector('#boton-1').addEventListener('click', function() {
        this.setAttribute("position", "0 100 -3");
        document.querySelector('#boton-2').setAttribute("position", "0 2 -3");
        document.querySelector('a-sphere').setAttribute("material", "src: pen/imagen/ciudad-360.jpg;");
    });
    document.querySelector('#boton-2').addEventListener('click', function() {
        this.setAttribute("position", "0 100 -3");
        document.querySelector('a-sphere').setAttribute("material", "src: pen/imagen/cubos-360.jpg;");
    });
</script>
```


## Crear nuestra primer Aplicación
