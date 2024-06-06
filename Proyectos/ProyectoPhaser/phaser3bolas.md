# Codigo que implementa un juego utilizando Phaser, una biblioteca de JavaScript para la creación de juegos 2D, junto con Synaptic, una biblioteca para redes neuronales. Aquí está el desglose de las funciones y variables clave:

## Variables Globales

- **Dimensiones del juego**:
  ```
  var w=800;
  var h=400;
  ```
  ### Define el ancho y la altura del área del juego.

- **Elementos del juego**:
  ```
  var jugador, fondo, bala, balaD=false, nave;
  var bala1, balaD1=false, nave1;
  var bala2, balaD2=false, nave2;
  var salto, adelante, menu;
  var velocidadBala,velocidadBala1,velocidadBala2;
  var despBala, despBala1, despBala2;
  var bala2desplazamientoHorizontal;
  var bala2desplazamientoVertical;
  var estatusAire, estatuSuelo, estatusDerecha, estatusIzquierda;
  var nnNetwork, nnEntrenamiento, nnSalida, datosEntrenamiento=[];
  var modoAuto = false, eCompleto=false;
  ```

## Inicialización del Juego

- **Juego Phaser**:
  ```
  var juego = new Phaser.Game(w, h, Phaser.CANVAS, '', { preload: preload, create: create, update: update, render: render });
  ```

## Funciones de Pre-carga y Creación

- **preload()**:
  ### Carga los activos del juego (imágenes y sprites).
  ```
  function preload() {
      juego.load.image('fondo', 'assets/game/fondo2.jpg');
      juego.load.spritesheet('mono', 'assets/sprites/altair.png',32 ,48);
      juego.load.image('nave', 'assets/game/ufo.png');
      juego.load.image('bala', 'assets/sprites/purple_ball.png');
      juego.load.image('menu', 'assets/game/menu.png');
  }
  ```

- **create()**:
  ### Inicializa la física del juego, crea los sprites y configura las teclas de control.
  ```
  function create() {
      juego.physics.startSystem(Phaser.Physics.ARCADE);
      juego.physics.arcade.gravity.y = 800;
      juego.time.desiredFps = 30;

      fondo = juego.add.tileSprite(0, 0, w, h, 'fondo');
      nave = juego.add.sprite(w-100, h-70, 'nave');
      bala = juego.add.sprite(w-100, h, 'bala');
      nave1 = juego.add.sprite(w-800, h-400, 'nave');
      bala1 = juego.add.sprite(w-760, h-380, 'bala');
      nave2 = juego.add.sprite(w-100, h-400, 'nave');
      bala2 = juego.add.sprite(w-100, h-370, 'bala');
      jugador = juego.add.sprite(50, h, 'mono');

      juego.physics.enable(jugador);
      jugador.body.collideWorldBounds = true;
      var corre = jugador.animations.add('corre',[8,9,10,11]);
      jugador.animations.play('corre', 10, true);
      
      juego.physics.enable(nave);
      nave.body.collideWorldBounds = true;
      juego.physics.enable(bala);
      bala.body.collideWorldBounds = true;
      juego.physics.enable(bala1);
      bala1.body.collideWorldBounds = true;
      juego.physics.enable(bala2);
      bala2.body.collideWorldBounds = true;

      pausaL = juego.add.text(w - 100, 20, 'Pausa', { font: '20px Arial', fill: '#fff' });
      pausaL.inputEnabled = true;
      pausaL.events.onInputUp.add(pausa, self);
      juego.input.onDown.add(mPausa, self);

      salto = juego.input.keyboard.addKey(Phaser.Keyboard.SPACEBAR);
      adelante = juego.input.keyboard.addKey(Phaser.Keyboard.RIGHT);

      nnNetwork =  new synaptic.Architect.Perceptron(6, 5, 5, 5, 4);
      nnEntrenamiento = new synaptic.Trainer(nnNetwork);
  }
  ```

## Funciones de la Red Neuronal

- **enRedNeural()**:
  ### Entrena la red neuronal con los datos de entrenamiento.
  ```
  function enRedNeural(){
      nnEntrenamiento.train(datosEntrenamiento, {rate: 0.0003, iterations: 10000, shuffle: true});
  }
  ```

- **datosDeEntrenamiento(param_entrada)**:
  ### Activa la red neuronal con los datos de entrada y evalúa si se debe mover a la derecha o izquierda.
  ```
  function datosDeEntrenamiento(param_entrada){
      nnSalida = nnNetwork.activate(param_entrada);
      var aire=Math.round( nnSalida[0]*100 );
      var piso=Math.round( nnSalida[1]*100 );
      var derecha=Math.round( nnSalida[2]*100 );
      var izquierda=Math.round( nnSalida[3]*100 );

      return nnSalida[2]>=nnSalida[3];
  }
  ```

- **EntrenamientoSalto(param_entrada)**:
  ### Activa la red neuronal y evalúa si se debe saltar.
  ```
  function EntrenamientoSalto(param_entrada){
      nnSalida = nnNetwork.activate(param_entrada);
      var aire=Math.round( nnSalida[0]*100 );
      var piso=Math.round( nnSalida[1]*100 );

      return nnSalida[0]>=nnSalida[1];
  }
  ```

## Funciones del Juego

- **pausa() y mPausa(event)**:
  ### Pausa y reanuda el juego.
  ```
  function pausa(){
      juego.paused = true;
      menu = juego.add.sprite(w/2,h/2, 'menu');
      menu.anchor.setTo(0.5, 0.5);
  }

  function mPausa(event){
      if(juego.paused){
          var menu_x1 = w/2 - 270/2, menu_x2 = w/2 + 270/2,
              menu_y1 = h/2 - 180/2, menu_y2 = h/2 + 180/2;

          var mouse_x = event.x  ,
              mouse_y = event.y  ;

          if(mouse_x > menu_x1 && mouse_x < menu_x2 && mouse_y > menu_y1 && mouse_y < menu_y2 ){
              if(mouse_x >=menu_x1 && mouse_x <=menu_x2 && mouse_y >=menu_y1 && mouse_y <=menu_y1+90){
                  eCompleto=false;
                  datosEntrenamiento = [];
                  modoAuto = false;
              }else if (mouse_x >=menu_x1 && mouse_x <=menu_x2 && mouse_y >=menu_y1+90 && mouse_y <=menu_y2) {
                  if(!eCompleto) {
                      enRedNeural();
                      eCompleto=true;
                  }
                  modoAuto = true;
              }

              resetVariables();
              resetVariables1();
              resetVariables2();
              resetPlayer();
              juego.paused = false;
              menu.destroy();
          }
      }
  }
  ```

- **resetVariables()**:
  ### Restablece las posiciones y velocidades de las balas.
  ```
  function resetVariables(){
      bala.body.velocity.x = 0;
      bala.position.x = w-100;
      balaD=false;
  }

  function resetVariables1(){
      bala1.body.velocity.y = -270;
      bala1.position.y = h-400;
      balaD1=false;
  }

  function resetVariables2(){
      bala2.body.velocity.y = -270;
      bala2.body.velocity.x = 0;
      bala2.position.x = w-100;
      bala2.position.y = h-500;
      balaD2=false;
  }
  ```

- **Movimiento del jugador**:
  ```
  function correr(){
      jugador.body.velocity.x = 150;
  }
  function saltar(){
      jugador.body.velocity.y = -270;
  }
  function Detenerse(){
      jugador.body.velocity.x = 0;
  }
  function retroceder(){
      jugador.body.velocity.x = -100;
  }
  function resetPlayer(){
      jugador.position.x=50;
  }
  ```

## Función de actualización

- **update()**:
 ### Actualiza el estado del juego en cada frame, maneja colisiones, controla el movimiento del jugador y las balas, y recopila datos de entrenamiento para la red neuronal.
  ```
  function update() {
      fondo.tilePosition.x -= 1; 

      juego.physics.arcade.collide(nave, jugador, colisionH, null, this);
      juego.physics.arcade.collide(bala, jugador, colisionH, null, this);
      juego.physics.arcade.collide(bala1, jugador, colisionH, null, this);
      juego.physics.arcade.collide(bala2, jugador, colisionH, null, this);

      estatuSuelo = 1;
      estatusAire = 0;

      estatusDerecha = 0;
      estatusIzquierda = 1;

      if(!jugador.body.onFloor() || jugador.body.velocity.y !=0) {
          estatuSuelo = 0;
          estatusAire = 1;
      }
      
      if(jugador.body.velocity.x >= 140

) {
          estatusDerecha = 1;
          estatusIzquierda = 0;
      }
      despBala = Math.floor(jugador.position.x - bala.position.x);
      despBala1 = Math.floor(jugador.position.x - bala1.position.x);
      bala2desplazamientoHorizontal = Math.floor(jugador.position.x - bala2.position.x);
      bala2desplazamientoVertical = Math.floor(jugador.position.y - bala2.position.y);
      despBala2 = Math.floor(bala2desplazamientoHorizontal + bala2desplazamientoVertical);

      if(modoAuto == false && salto.isDown && !estatusAire){
          if(jugador.body.velocity.x <= 0){
              jugador.body.velocity.x = 150;
              saltar();
              correr();
          }
          else{ 
              saltar();
              Detenerse();
          }
      }
      if(modoAuto == false && adelante.isDown && jugador.body.onFloor()){
          correr();
      }
      if(modoAuto == false && !adelante.isDown && jugador.body.onFloor() && jugador.position.x == 50){
          Detenerse();
      }
      if(modoAuto == false && jugador.body.onFloor() && jugador.position.x >= 150){
          retroceder();
      }

      if(modoAuto == true && bala.position.x > 0 && jugador.body.onFloor()){
          if(EntrenamientoSalto([despBala , velocidadBala, despBala1, velocidadBala1, despBala2, velocidadBala2])){
              if(jugador.body.velocity.x <= 0){
                  jugador.body.velocity.x = 150;
                  saltar();
                  correr();
              }
              else{ 
                  saltar();
                  Detenerse();
              }
          }
          if(datosDeEntrenamiento([despBala , velocidadBala, despBala1, velocidadBala1, despBala2, velocidadBala2])){
              correr();         
          }else if(jugador.body.onFloor() && jugador.position.x >= 150){
              Detenerse();
              retroceder();
          }
      }

      if(balaD == false){
          disparo();
      }
      if(balaD1 == false){
          disparo1();
      }
      if(balaD2 == false){
          disparo2();
      }

      if(bala.position.x <= 0){
          resetVariables();
      }
      if(bala1.position.y >= 355){
          resetVariables1();
      }
      if(bala2.position.x <= 0 || bala2.position.y >= 355){
          resetVariables2();
      }
  
      if(modoAuto == false && bala.position.x > 0){
          datosEntrenamiento.push({
              'input' : [despBala , velocidadBala, despBala1, velocidadBala1, despBala2, velocidadBala2],
              'output': [estatusAire , estatuSuelo, estatusDerecha, estatusIzquierda]  
          });
      }
  }
  ```

## Funciones de disparo y colisión

- **disparo()**:
  ### Controla el movimiento de las balas.
  ```
  function disparo(){
      velocidadBala = -1 * velocidadRandom(300,700);
      bala.body.velocity.y = 0 ;
      bala.body.velocity.x = velocidadBala ;
      balaD=true;
  }

  function disparo1(){
      velocidadBala1 = -1 * velocidadRandom(300,800);
      bala1.body.velocity.y = 0 ;
      balaD1=true;
  }

  function disparo2(){
      velocidadBala2 = -1 * velocidadRandom(400,500);
      bala2.body.velocity.y = 0 ;
      bala2.body.velocity.x = 1.60*velocidadBala2 ;
      balaD2=true;
  }
  ```

- **colisionH()**:
  ### Maneja la colisión entre el jugador y otros elementos.
  ```
  function colisionH(){
      pausa();
  }
  ```

- **velocidadRandom(min, max)**:
  ### Genera una velocidad aleatoria para las balas.
  ```
  function velocidadRandom(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
  }
  ```

## Función de renderizado

- **render()**:
  ### Función de renderizado que está vacía, pero se podría usar para dibujar elementos adicionales.
  ```
  function render(){

  }
  ```

### Este código crea un juego donde un jugador esquiva balas controladas por la física de Phaser. Además, implementa una red neuronal para entrenar al jugador a esquivar automáticamente las balas basándose en datos de entrenamiento recopilados durante el juego.