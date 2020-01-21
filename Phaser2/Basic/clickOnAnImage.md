# Click on an image

```javascript

var game = new Phaser.Game(800, 600, Phaser.AUTO, 'phaser-example', { preload: preload, create: create });

var text;
var counter = 0;

function preload () {
    game.load.image('einstein', 'assets/pics/ra_einstein.png');
}

function create() {
    var image = game.add.sprite(game.world.centerX, game.world.centerY, 'einstein');
    image.anchor.set(0.5);
    image.inputEnabled = true;

    text = game.add.text(250, 16, '', { fill: '#ffffff' });

    image.events.onInputDown.add(listener, this);
}

function listener () {

    counter++;
    text.text = "You clicked " + counter + " times!";

}

```

### 

## preload()

```javascript
function preload () {
  	// 첫 번째 파라미터: 코드에서 사용될 유니크한 이름
  	// 두 번째 파라미터: 이미지의 url
    game.load.image('einstein', 'assets/pics/ra_einstein.png');
}
```

preload 함수에서는 image, sprite 등을 불러온다.



## create()

```javascript
function create() {
    var image = game.add.sprite(game.world.centerX, game.world.centerY, 'einstein');
    image.anchor.set(0.5);
    image.inputEnabled = true;

    text = game.add.text(250, 16, '', { fill: '#ffffff' });

    image.events.onInputDown.add(listener, this);
}
```

create 함수는 preload가 실행된 후에 실행된다.

gameObjectFactory는 game.add를 통해 관리할 수 있다.

game 객체에 sprite, audio, filter, group, text, tween, video 등을 **add** 할 수 있다.



### Sprite

properties: alive, anchor, angle, body, events, input, inCamera, texture, inputEnabled 등

#### anchor

> 텍스처의 origin point를 설정한다.

* default: 0, 0
* anchor.set(0.5) == anchor.set(0.5, 0.5) : 센터
* 1, 1 : 오른쪽 아래

#### inputEnabled

> 기본적으로 게임 객체는 어떠한 input event 도 허용하지 않지만 이것을 통해 input을 받을 수 있다. 

* `this.input` 으로 input handler 에 접근할 수 있다. 

* `this.events` ( i.e, events.onInputDown) 과 같이 잠시 input을 불가능하게 만들고 싶다면? 

  `input.enabled = false` 로 설정해주자

#### text

> text object 를 만들어준다.



### image.events.onInputDown.add()

