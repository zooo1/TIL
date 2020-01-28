# Image follow input

```javascript

var game = new Phaser.Game(800, 600, Phaser.AUTO, 'phaser-example', { preload: preload, create: create, update: update, render: render });

function preload() {

    game.load.image('phaser', 'assets/sprites/phaser.png');

}

var sprite;

function create() {

    game.physics.startSystem(Phaser.Physics.ARCADE);

    sprite = game.add.sprite(game.world.centerX, game.world.centerY, 'phaser');
    sprite.anchor.set(0.5);

    game.physics.arcade.enable(sprite);

}

function update () {

    if (game.physics.arcade.distanceToPointer(sprite, game.input.activePointer) > 8)
    {
        //  Make the object seek to the active pointer (mouse or touch).
        game.physics.arcade.moveToPointer(sprite, 300);
    }
    else
    {
        //  Otherwise turn off velocity because we're close enough to the pointer
        sprite.body.velocity.set(0);
    }

}

function render () {

	game.debug.inputInfo(32, 32);

}

```

