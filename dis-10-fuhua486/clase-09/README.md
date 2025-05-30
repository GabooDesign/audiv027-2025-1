# clase-09

viernes 09 mayo 2025

## TeachSeñas

integrantes:

* Fuhua Huang <LINK [A GITHUB](https://github.com/fuhua486) >
* Ignacio Castro C. <LINK [A GITHUB](https://github.com/nachofau)>

```md
mi equipo de trabajo es <https://github.com/fuhua486> y <https://github.com/nachofau>, entregamos en el repositorio en este enlace <https://github.com/ETC>.
```

## acerca del proyecto

El proyecto TeachSeñas(Teach de Teachable Machine, Señas:Lengua de Señas Chilena) se consiste en reconocer y entender palabras básicas de la Lengua de Señas Chilena (LSCh), como "Hola", "Gracias", "Sí", "No", "Casa","Amor","Amigo"y "Por favor". Ayudando a establecer una comunicacion basica entre persona que usa lengua de señas chilena y persona que no entiende nada acerca de esta lenguaje, tambien ayuda las personas a aprender señas esenciales de manera interactiva.(porque para interactuar, tiene que saber la lengua de seña chilena, no todos, pero en los ejemplos que usamos como base para el proyecto).  
El proyecto funciona a través de una cámara: el usuario hace una seña con las manos, y el sistema los reconoce en tiempo real y muestra su significado(en palabras) en la pantalla.

Las herramientas que utilizamos son: **ml5.js**, **p5.js Web Editor** y **Teachable Machine**.

![image](https://github.com/user-attachments/assets/2bbe7c2d-6e6d-47ad-b3b8-20ffbd7ca4fa)
![image](https://github.com/user-attachments/assets/0cc6c770-3580-4f7e-9302-97539654adb2)
![image](https://github.com/user-attachments/assets/1aef7aac-9d69-4c1c-8e68-a9bd9ca6a6c9)

Como herramienta que utilizamos a menudo en nuestros aprendizajes, ml5.js nos proporciona muchos modelos para utilizar.

En la clase de Inteligencia Artificial de profe Aarón Montoya, nos mostró las funciones de los dos modelos, handpose e ImageClassifier, y explicó cómo utilizar el código con el apoyo de ayudante.

Basándonos en esto, a continuación mostraremos el proceso de creación de nuestro proyecto.

## procesos

Para comenzar, analizamos las 12 palabras en Lengua de Señas Chilena más importantes según la fuente de biobioChile. (https://www.biobiochile.cl/noticias/servicios/toma-nota/2024/09/24/12-palabras-en-lengua-de-senas-chilena-que-deberias-aprender-te-sabes-alguna.shtml)

Dividimos estas palabras en dos categorias: Con movimiento(dinamico)/Sin movimientos(estatico)

Las palabras con movimientos son: Hola, Gracias, Por favor, Si, No, Amigo/a, Perdón, Comida y Agua.
Las palabras sin movimientos(Estatico) son: Casa y Amor.
![image](https://github.com/user-attachments/assets/737b78f8-ea2a-447c-8d5d-9b7fc0414963)

Después de clasificar los movimientos, vamos a entrenar nuestro modelo en Teachable Machine.
Al principio probamos el modelo de postura, pero no nos resulta, porque el modelo no se puede detectar las diferencias entre la postura de mano y dedos, el modelo solo sirve para detectar postura en el sentido mas amplio(movimiento con todos el cuerpo), y no en detalles pequeños.

![image](https://github.com/user-attachments/assets/cd1f05bb-cd97-4e55-85ac-747ecb1ba0f0)


Luego vamos a usar el modelo de imagen, probando diferentes lengua de seña. El modelo de imagen resulta mucho mejor que de postura para nuestro proyecto, pero se requiere un tamaño de muestra mayor para detectarlo mejor.
El modelo de imagen de Teachable Machine se ve afectado por muchos factores a la hora de distinguir el lenguaje de señas: la distancia del gesto, la apariencia del demostrador (mostrando solo el cuerpo o la persona completa), el entorno y el color de la ropa.

Intentamos todo eso de mostrar la lengua de señas chilena delante de la cámara: sí, no, gracias. El resultado es que Teachable Machine no puede reconocer con precisión el lenguaje de señas que expresamos(a una distancia que se puede ver el demostrador y el gesto que hace). Sólo puede reconocerlo cuando mostramos los gestos individualmente y estamos cerca de los gestos en lugar de la persona(el demostrador). Además, no es 100% exacto y hay algunas desviaciones. Por esta razón, terminamos eligiendo solo ocho frases para la capacitación: Hola, Gracias, Por favor, Si, No, Amigo/a, Casa y Amor.

![image](https://github.com/user-attachments/assets/fc883f9a-bb75-4b30-8e54-c0315d1e4a4e)


Después de grabar las muestras de imagenes con el Webcam, entramos la face de preparación de modelo, y exportar el modelo para usarlo en nuestro proyecto.
Cuando termina de exportar el modelo, Teachable Machine nos entregó un link, que sirve para usar en el modelo **Image + Teachable Machine** de ml5.

![image](https://github.com/user-attachments/assets/c9e86160-c928-464e-84af-1cd531dadc69)

## código del proyecto

**el código original que citamos es**

```javascript
// A variable to initialize the Image Classifier
let classifier;

// A variable to hold the video we want to classify
let video;

// Variable for displaying the results on the canvas
let label = "Model loading...";

let imageModelURL = "https://teachablemachine.withgoogle.com/models/bXy2kDNi/";

function preload() {
  ml5.setBackend('webgl');
  classifier = ml5.imageClassifier(imageModelURL + "model.json");
}

function setup() {
  createCanvas(640, 480);

  // Create the webcam video and hide it
  video = createCapture(VIDEO, { flipped: true });
  video.size(640, 480);
  video.hide();

  // Start detecting objects in the video
  classifier.classifyStart(video, gotResult);
}

function draw() {
  // Each video frame is painted on the canvas
  image(video, 0, 0);

  // Printing class with the highest probability on the canvas
  fill(0, 255, 0);
  textSize(32);
  text(label, 20, 50);
}

// A function to run when we get the results
function gotResult(results) {
  // Update label variable which is displayed on the canvas
  label = results[0].label;
}
```

**Modificaciones en el código:**
```javascript
// Aquí reemplazamos el enlace original con el enlace del modelo Teachable Machine entrenado
let imageModelURL = "https://teachablemachine.withgoogle.com/models/0Gt6Qh-9H/";
```

```javascript
  // En el fill modificamos el color de texto a Naranjo
  fill(241, 111, 36);
  // Cambiamos el tamaño de las letras(texto),de 35 a 50.
  textSize(50);
  //Aquí aprendimos en el tutorial que textAlign se usa para alinear texto.
  textAlign(CENTER,BOTTOM)
  //Aquí aprendimos que podemos usar directamente la división y números específicos para cambiar la posición del texto.
  text(label, width/2, height-10);
```
**Código final(Resultados):**
```javascript
// A variable to initialize the Image Classifier
let classifier;

// A variable to hold the video we want to classify
let video;

// Variable for displaying the results on the canvas
let label = "Model loading...";

// Aquí reemplazamos el enlace original con el enlace del modelo Teachable Machine entrenado
let imageModelURL = "https://teachablemachine.withgoogle.com/models/0Gt6Qh-9H/";

function preload() {
  ml5.setBackend('webgl');
  classifier = ml5.imageClassifier(imageModelURL + "model.json");
}

function setup() {
  createCanvas(640, 480);

  // Create the webcam video and hide it
  video = createCapture(VIDEO, { flipped: true });
  video.size(640, 480);
  video.hide();

  // Start detecting objects in the video
  classifier.classifyStart(video, gotResult);
  
}

function draw() {
  // Each video frame is painted on the canvas
  image(video, 0, 0);

  // En el fill modificamos el color de texto a Naranjo
  fill(241, 111, 36);
  // Cambiamos el tamaño de las letras(texto),de 35 a 50.
  textSize(50);
  //Aquí aprendimos en el tutorial que textAlign se usa para alinear texto.
  textAlign(CENTER,BOTTOM)
  //Aquí aprendimos que podemos usar directamente la división y números específicos para cambiar la posición del texto.
  text(label, width/2, height-10);
  
}

// A function to run when we get the results
function gotResult(results) {
  // Update label variable which is displayed on the canvas
  label = results[0].label;
}

```

## enlace del proyecto

lo hicimos en editor de p5.js: https://editor.p5js.org/fuhua486/sketches/mbhcL3rEE

## documentación multimedia / audiovisual del proyecto funcionando

Registro Teachable Machine:
https://youtube.com/shorts/mPrGFbh1_Ns?feature=share

Prueba de modelo p5js (Teachable machine+ modelo handpose) : 
https://youtu.be/pjYUg1VZ7hY

## bibliografía

Nos basamos en el tutorial de [The Coding Train](https://youtu.be/kwcillcWOg0?si=LD4fNH_918367TAY) :
Teachable Machine 1: Image Classification

Usamos el modelo **ml5 + Teachable Machine: Image + Teachable Machine** en la biblioteca ml5.js, luego tomamos el código base en [p5.js](https://editor.p5js.org/ml5/sketches/VvGXajA36) para hacer el proyecto

Fuentes de información sobre Lengua de Señas Chilena usada en nuestro proyecto: https://www.biobiochile.cl/noticias/servicios/toma-nota/2024/09/24/12-palabras-en-lengua-de-senas-chilena-que-deberias-aprender-te-sabes-alguna.shtml

## conclusiones

Teachable Machine es muy práctico, pero necesitábamos entrenar el modelo intensivamente para lograr los resultados que queríamos. Como explicamos en el proceso, los modelos de "Imagen" y “Postura” de Teachable Machine se ven afectados por muchos factores durante el entrenamiento, lo que afectó en cierta medida nuestro estado de ánimo (el modelo de postura se utilizó en primera instancia pero no era adecuado para nuestro proyecto). Al final nos decantamos por el de imagen el cual después de ser entrenado con cientos de muestras de imágenes ,la precisión del modelo mejoró en el reconocimiento del lenguaje de señas.

Creemos que el modelo puede seguir entrenándose hasta que pueda reconocer con precisión y rapidez más lengua de señas chilena, facilitando la comunicación entre personas que usan lengua de señas chilena y quienes no entienden lengua de señas, haciendo de la inteligencia artificial una herramienta para facilitarnos la vida. Al mismo tiempo, también debemos sopesar la relación entre "los usuarios necesitan adaptarse a las herramientas" y "las herramientas necesitan adaptarse a los usuarios".

No tuvimos muchos problemas con el código ml5 porque nos proporcionó un modelo casi funcional desde el principio. Luego proporcionamos el tutorial de The Coding Train para aprender a usar Teachable Machine con ml5 y p5js. Después de modificar el código sobre la vinculación del modelo y el ajuste del texto, se logró utilizar normalmente.

En general, disfrutamos mucho de este Proyecto, especialmente el momento en que el modelo funcionó correctamente, y también aprendimos muchos conocimientos nuevos y comprendimos la diversidad de la programación, como también podemos aprender e inspirarnos con el trabajo de la comunidad de programación para resolver dudas y mejorar nuestro proyecto.

¡Muchas gracias!
