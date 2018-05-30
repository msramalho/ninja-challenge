<p align="center"><img src="https://i.imgur.com/cSV2br2.png"></p>

<h3 align="center">This repo contains some thoughts belonging to <i>maps</i> aka <i><a href="https://github.com/msramalho">msramalho</a></i> on the problems of the 
<a href="https://ninjachallenge.jscrambler.com/">ninja challenge 2018</a> by 
<a href="https://jscrambler.com/">jscrambler</a></h3>


# 1 - Shroud of Concealment
### Problem
Write code that executes the same as the following code in the **least amount of characters**:
```javascript
function seq(ȶȑ){
    var ˠビ=0,ˡɃ=ˠビ,ヘɍ='',ɀʶ=10,ӷベ=ˡɃ+1,ȱˁ='length',ȿϧ=ȶȑ&&ˡɃ+ɀʶ;
    function ʲヒ(){ˠビ=ӷベ;;}
    function ブʱ(){ӷベ=ȿϧ;}
    function べϼ(){ˡɃ+=パひ(ʲヒ,ヘɍ)[ȱˁ]-パひ(ヘɍ,ブʱ)[ȱˁ]}
    function ɂふ(){return ˠビ+ӷベ}
    function ʹϡ(ӷベ){return パひ(ɀʶ*(ɀʶ),ɂふ())}
    function ӿѩ(Ͼぱ){return ˡɃ===ɀʶ*Ͼぱ}
    function ˢぺ(Ͼぱ){べϼ();ȿϧ=ӿѩ(Ͼぱ)?ʹϡ(ˡɃ):ɂふ();(ʲヒ(),ブʱ());}
    function パひ(ʲヒ,ブʱ,べϼ){return べϼ?ʲヒ||ブʱ:(ブʱ)+ʲヒ;}
    while(ˡɃ<ȶȑ){ˢぺ(ȱˁ[(ȱˁ)]-パひ(ヘɍ,ɀʶ)[ȱˁ]);}
    return パひ(ȿϧ,パひ(ブʱ,ヘɍ)[ȱˁ]-パひ(ヘɍ,ブʱ)[ȱˁ],パひ);
}
```
### Rationale 


# 2 - Don’t mess with my Shuriken
### Problem
Replace `<OUR CODE GOES HERE>` by a condition that allows the ninja to detect that there was some sabotage. The ninja will perform 5 weapon attacks, reload, and attack 5 times once more, so as to kill both enemies (`DrunkenFist` and `FlyingPunch`) after 10 throws.
```javascript
'use strict';

var EnemyNinja = function(name) {
    return (function() {
        var _health = 100;
        var _name = name;

        return Object.freeze({ // no properties can be added, nothing changes
            getName: function() {
                return '' + _name;
            },

            getHealth: function() {
                return _health;
            },

            takesDamage: function(damage) {
                _health -= damage;
                return _health;
            }
        });
    })();
};

var DrunkenFist = new EnemyNinja('Drunken Fist');
var FlyingPunch = new EnemyNinja('Flying Punch');

var Shuriken = (function() {

    var _energy = 100;
    var ENERGY_UNIT = 1,
        ENERGY_ATTACK_COST = ENERGY_UNIT * 20;
    var DAMAGE_ON_HIT = 20;

    return Object.freeze({

        recharge: function fn() {

            if (
                // <OUR CODE GOES HERE>
            ) {
                throw 'Weapon-tampering-detected-o-jutsu!';
            }

            if (_energy <= 100) {
                _energy += ENERGY_UNIT;
            }
        },

        'throw': function(enemyTarget) {
            var _weaponThrown = false;

            if (_energy > 0) {
                _energy -= ENERGY_ATTACK_COST;
                enemyTarget.takesDamage(DAMAGE_ON_HIT);
                _weaponThrown = true;
            }

            return _weaponThrown;
        }
    });
})();
```
### Rationale

```javascript

for (let i = 0; i < 5; i++) {
    Shuriken.throw(DrunkenFist);
    console.log(DrunkenFist.getHealth());
}
for (let i = 0; i < 101; i++) {
    Shuriken.recharge();
}
for (let i = 0; i < 5; i++) {
    Shuriken.throw(FlyingPunch);
    console.log(FlyingPunch.getHealth());
}
```

# 3 - Steal the Jewels by Claranet
### Problem
### Rationale 

# 4 - Antidotes for the Trip
### Problem
### Rationale 
