# Move an Image

```javascript
var game = new Phaser.Game(800, 600, Phaser.AUTO, 'phaser-example', { preload: preload, create: create });

function preload() {
    game.load.image('einstein', 'assets/pics/ra_einstein.png');
}

function create() {

    var image = game.add.sprite(0, 0, 'einstein');

    game.physics.enable(image, Phaser.Physics.ARCADE);

    image.body.velocity.x=150;

}

```



```javascript
function create() {

    var image = game.add.sprite(0, 0, 'einstein');
  	
  	game.physics.enable(image, Phaser.Physics.ARCADE);

    image.body.velocity.x=150;

}
```



## game.add.sprite(x, y, name)

로딩된 이미지를 불러와서 하나의 sprite 객체로 만든다.



## physics

> game의 public property

public methods: clear, destroy, enable, parseConfig, preUpdate, reset, setBoundToWorld, update 등

### enable(object, system, debug)

> game object 또는 object 배열에 physics body를 생성한다.

* object: physics body가 생성될 게임 객체

* system: Phaser.Physics.ARCADE, Phaser.Physics.P2JS, NINJA, MATTER 등이 있음

```javascript
image.body.velocity.x = 10;
```

* image가 이동하는 효과(속도를 넣어준다.)

* game.sprite.body => Phaser.Physics.Arcade.Body

* default 값으로는 아무것도 없다.

* 이곳에 접근하기 위해서는 game.physics.enable(object, system) 을 호출해주어야 한다.

* game.sprite.body의 properties: acceleration, allowGravity, game, gravity, left, right, immovable, velocity, touching, x, y 등

​    