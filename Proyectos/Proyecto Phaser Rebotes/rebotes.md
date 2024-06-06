# Codigo de phaser rebotes.

### Constantes y Variables:
- Se definen varias constantes como el ancho (`WIDTH`) y alto (`HEIGHT`) del lienzo del juego, la velocidad del jugador (`PLAYER_SPEED`), la velocidad de la pelota (`BALL_SPEED`), el texto para la pausa (`PAUSE_TEXT`), y el estilo para el texto de pausa (`PAUSE_STYLE`).
- También se inicializan variables para los elementos del juego como el jugador (`player`), el fondo (`background`), la pelota (`ball`), la etiqueta de pausa (`pauseLabel`), y el menú de pausa (`menu`), así como para el control del teclado (`cursors`) y otras relacionadas con el entrenamiento de una red neuronal (`nnNetwork`, `nnTrainer`, `nnOutput`, `trainingData`).
- Además, se definen variables para el modo automático (`isAutoMode`), si el entrenamiento está completo (`isTrainingComplete`), y para el retorno del jugador al centro (`returningV`, `returningH`), y para las coordenadas del jugador (`JX`, `JY`).
- Se crea una instancia del juego Phaser con las dimensiones definidas y los métodos de preload, create, update, y render.

### Funciones de Precarga (`preload()`):
- Se carga la imagen de fondo, el spritesheet del jugador, el menú de pausa y la imagen de la pelota.

### Función de Creación (`create()`):
- Se inicia el sistema de físicas de ARCADE de Phaser y se deshabilita la gravedad para permitir el movimiento libre.
- Se establece la velocidad de fotogramas por segundo (`FPS`).
- Se crea el fondo, el jugador y la pelota.
- Se habilitan las físicas para el jugador y se establece que colisione con los límites del mundo.
- Se establece una animación para el jugador.
- Se habilitan las físicas para la pelota y se establece que rebote al colisionar con los límites del mundo.
- Se establece una velocidad aleatoria para la pelota.
- Se crea y configura la etiqueta de pausa.
- Se asignan manejadores de eventos para los clics del ratón y las teclas de dirección.

### Función para Establecer Velocidad Aleatoria de la Pelota (`setRandomBallVelocity()`):
- Se genera un ángulo aleatorio y se utiliza para establecer la velocidad de la pelota en una dirección aleatoria.

### Función para Pausar el Juego (`pauseGame()`):
- Se pausa el juego y se muestra el menú de pausa en el centro de la pantalla.

### Función para Manejar los Clics del Ratón en el Menú de Pausa (`handlePauseClick(event)`):
- Se maneja el evento de clic del ratón en el menú de pausa.
- Se verifica si se hizo clic en alguna de las opciones del menú (restablecer entrenamiento o continuar entrenamiento).
- Se realizan acciones según la opción seleccionada.

### Función para Restablecer el Entrenamiento (`resetTraining()`):
- Se restablece el estado del entrenamiento de la red neuronal.

### Función para Restablecer el Juego (`resetGame()`):
- Se restablece el estado del juego colocando al jugador y la pelota en posiciones iniciales aleatorias.

### Función para Actualizar el Juego (`update()`):
- Se maneja la lógica del juego en cada cuadro, como el movimiento del jugador, la detección de colisiones y la lógica de la red neuronal.

### Función para Manejar la Entrada del Jugador (`handlePlayerInput()`):
- Se maneja la entrada del jugador desde el teclado y se ajusta la velocidad del jugador en consecuencia.

### Función para Almacenar Datos de Entrenamiento (`storeTrainingData(dx, dy, distance)`):
- Se almacenan los datos de entrenamiento para la red neuronal.

### Función para Evaluar el Movimiento (`evaluateMovement(input)`):
- Se evalúa si el movimiento automático debe activarse basado en la salida de la red neuronal.

### Función para Controlar el Movimiento Automático del Jugador (`autoMovePlayer(dx, dy, distance)`):
- Se controla el movimiento automático del jugador según las entradas y la salida de la red neuronal.

### Funciones Auxiliares para Evaluar el Movimiento Vertical y Horizontal (`evaluateVerticalMovement(input)`, `evaluateHorizontalMovement(input)`):
- Se evalúa el movimiento vertical y horizontal basado en la salida de la red neuronal.

### Función para Ajustar la Velocidad del Jugador (`adjustPlayerVelocity(verticalMove, horizontalMove, distance)`):
- Se ajusta la velocidad del jugador en función del movimiento vertical y horizontal.

### Función para Manejar el Retorno del Jugador al Centro (`handleReturnToCenter()`):
- Se maneja el retorno del jugador al centro del escenario cuando se mueve demasiado hacia los bordes.

### Función para Manejar la Colisión entre el Jugador y la Pelota (`handleCollision()`):
- Se activa el modo automático y se pausa el juego al ocurrir una colisión entre el jugador y la pelota.

### Función de Representación Opcional (`render()`):
- Se representa opcionalmente el estado del juego o información adicional.

 ```
const WIDTH = 400;
const HEIGHT = 400;
const FPS = 30;
const PLAYER_SPEED = 300;
const BALL_SPEED = 550;
const PAUSE_TEXT = 'Pausa';
const PAUSE_STYLE = { font: '20px Arial', fill: '#fff' };

var player, background, ball, pauseLabel, menu;
var cursors, nnNetwork, nnTrainer, nnOutput, trainingData = [];
var isAutoMode = false, isTrainingComplete = false;
var returningV = false, returningH = false;
var JX = 250, JY = 250;

var game = new Phaser.Game(WIDTH, HEIGHT, Phaser.CANVAS, '', {
    preload: preload,
    create: create,
    update: update,
    render: render
});

function preload() {
    game.load.image('background', 'assets/game/fondo1.jpeg');
    game.load.spritesheet('player', 'assets/sprites/altair.png', 32, 48);
    game.load.image('menu', 'assets/game/menu.png');
    game.load.image('ball', 'assets/sprites/purple_ball.png');
}

function create() {
    game.physics.startSystem(Phaser.Physics.ARCADE);
    game.physics.arcade.gravity.y = 0; // No gravity for free movement
    game.time.desiredFps = FPS;
    background = game.add.tileSprite(0, 0, WIDTH, HEIGHT, 'background');
    player = game.add.sprite(WIDTH / 2, HEIGHT / 2, 'player');
    game.physics.enable(player);
    player.body.collideWorldBounds = true;
    var run = player.animations.add('run', [8, 9, 10, 11]);
    player.animations.play('run', 10, true);
    ball = game.add.sprite(0, 0, 'ball');
    game.physics.enable(ball);
    ball.body.collideWorldBounds = true;
    ball.body.bounce.set(1);
    setRandomBallVelocity();
    pauseLabel = game.add.text(WIDTH - 100, 20, PAUSE_TEXT, PAUSE_STYLE);
    pauseLabel.inputEnabled = true;
    pauseLabel.events.onInputUp.add(pauseGame, this);
    game.input.onDown.add(handlePauseClick, this);
    cursors = game.input.keyboard.createCursorKeys();
    nnNetwork = new synaptic.Architect.Perceptron(3, 6, 6, 6, 5);
    nnTrainer = new synaptic.Trainer(nnNetwork);
}

function setRandomBallVelocity() {
    var angle = game.rnd.angle();
    // var angle = 100;
    ball.body.velocity.set(Math.cos(angle) * BALL_SPEED, Math.sin(angle) * BALL_SPEED);
}

function pauseGame() {
    game.paused = true;
    menu = game.add.sprite(WIDTH / 2, HEIGHT / 2, 'menu');
    menu.anchor.setTo(0.5, 0.5);
}

function handlePauseClick(event) {
    if (game.paused) {
        var menuBounds = {
            x1: WIDTH / 2 - 270 / 2,
            x2: WIDTH / 2 + 270 / 2,
            y1: HEIGHT / 2 - 180 / 2,
            y2: HEIGHT / 2 + 180 / 2
        };
        if (event.x > menuBounds.x1 && event.x < menuBounds.x2 && event.y > menuBounds.y1 && event.y < menuBounds.y2) {
            if (event.y <= menuBounds.y1 + 90) {
                resetTraining();
            } else if (event.y > menuBounds.y1 + 90) {
                if (!isTrainingComplete) {
                    console.log("","Entrenamiento "+ trainingData.length +" valores" );
                    nnTrainer.train(trainingData, { rate: 0.0003, iterations: 10000, shuffle: true });
                    isTrainingComplete = true;
                }
                isAutoMode = true;
            }
            menu.destroy();
            resetGame();
            game.paused = false;
        }
    }
}

function resetTraining() {
    isTrainingComplete = false;
    trainingData = [];
    isAutoMode = false;
}

function resetGame() {
    player.x = WIDTH / 2;
    player.y = HEIGHT / 2;
    player.body.velocity.x = 0;
    player.body.velocity.y = 0;

    ball.x = 0;
    ball.y = 0;
    setRandomBallVelocity();
}

function update() {


    if (!isAutoMode) {
        handlePlayerInput();
    }

    game.physics.arcade.collide(ball, player, handleCollision, null, this);

    var dx = ball.x - player.x;
    var dy = ball.y - player.y;
    // distancia euclidiana entre ellos
    var distance = Math.sqrt(dx * dx + dy * dy);

    if (!isAutoMode) {
        storeTrainingData(dx, dy, distance);
    } else if (isAutoMode && evaluateMovement([dx, dy, distance, JX, JY])) {
        autoMovePlayer(dx, dy, distance);
    }

    
}

function handlePlayerInput() {
    player.body.velocity.x = 0;
    player.body.velocity.y = 0;

    if (cursors.left.isDown) {
        player.body.velocity.x = -PLAYER_SPEED;
    } else if (cursors.right.isDown) {
        player.body.velocity.x = PLAYER_SPEED;
    }

    if (cursors.up.isDown) {
        player.body.velocity.y = -PLAYER_SPEED;
    } else if (cursors.down.isDown) {
        player.body.velocity.y = PLAYER_SPEED;
    }
}

function storeTrainingData(dx, dy, distance) {
    var left = 0, right = 0, up = 0, down = 0, moving = 0;

    if (dx > 0) left = 1;
    else right = 1;

    if (dy > 0) up = 1;
    else down = 1;

    if (player.body.velocity.x != 0 || player.body.velocity.y != 0) moving = 1;

    JX = player.x;
    JY = player.y;

    trainingData.push({
        'input': [dx, dy, distance, JX, JY],
        'output': [left, right, up, down, moving]
    });

    console.log(
                "Diferencia de la posicion de la bala contra la del jugador en X: ", dx + "\n"+
                "Diferencia de la posicion de la bala contra la del jugador en Y: ", dy + "\n"+
                "Distancia euclidiana entre la bola y el jugador: ", distance + "\n"+
                "Posicion del jugador en X: ", JX + "\n"+
                "Posicion del jugador en Y: ", JY + "\n"
 
            );
    console.log(
                "Izquierda: ", left + "\n"+
                "Derecha: ", right + "\n"+
                "Arriba: ", up + "\n"+
                "Abajo: ", down + "\n"+
                "Movimiento: ", moving + "\n"
            );
}
function evaluateMovement(input) {
    nnOutput = nnNetwork.activate(input);
    return nnOutput[4] * 100 >= 20;
}
function autoMovePlayer(dx, dy, distance) {
    if (distance <= 150) {
        var verticalMove = evaluateVerticalMovement([dx, dy, distance, JX, JY]);
        var horizontalMove = evaluateHorizontalMovement([dx, dy, distance, JX, JY]);
        adjustPlayerVelocity(verticalMove, horizontalMove, distance);
        handleReturnToCenter();
    } else if (distance >= 200) {
        player.body.velocity.x = 0;
        player.body.velocity.y = 0;
    }
}
function evaluateVerticalMovement(input) {
    nnOutput = nnNetwork.activate(input);
    return nnOutput[2] >= nnOutput[3];
}
function evaluateHorizontalMovement(input) {
    nnOutput = nnNetwork.activate(input);
    return nnOutput[0] >= nnOutput[1];
}
function adjustPlayerVelocity(verticalMove, horizontalMove, distance) {
    if (verticalMove && !returningV) {
        player.body.velocity.y -= 35;
    } else if (!verticalMove && !returningV && distance <= 105) {
        player.body.velocity.y += 35;
    }

    if (horizontalMove && !returningH) {
        player.body.velocity.x -= 35;
    } else if (!horizontalMove && !returningH && distance <= 105) {
        player.body.velocity.x += 35;
    }
}
function handleReturnToCenter() {
    // Si el jugador está demasiado a la derecha o demasiado a la izquierda
    if (player.x > 350) {
        // Mover al jugador hacia la izquierda (hacia el centro)
        player.body.velocity.x = -200; // Velocidad negativa en el eje x
        returningH = true;
    } else if (player.x < 50) {
        // Mover al jugador hacia la derecha (hacia el centro)
        player.body.velocity.x = 200; // Velocidad positiva en el eje x
        returningH = true;
    } else if (returningH && (player.x >= 50 && player.x <= 350)) {
        // Si el jugador está en el rango correcto en el eje x, detener el movimiento horizontal
        player.body.velocity.x = 0;
        returningH = false;
    }
    // Si el jugador está demasiado arriba o demasiado abajo
    if (player.y > 350) {
        // Mover al jugador hacia arriba (hacia el centro)
        player.body.velocity.y = -200; // Velocidad negativa en el eje y
        returningV = true;
    } else if (player.y < 50) {
        // Mover al jugador hacia abajo (hacia el centro)
        player.body.velocity.y = 200; // Velocidad positiva en el eje y
        returningV = true;
    } else if (returningV && (player.y >= 50 && player.y <= 350)) {
        // Si el jugador está en el rango correcto en el eje y, detener el movimiento vertical
        player.body.velocity.y = 0;
        returningV = false;
    }
}
function handleCollision() {
    isAutoMode = true;
    pauseGame();
}
function render() {
    // Optionally, render game state or additional information
}

  ```