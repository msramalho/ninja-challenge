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

https://javascript-minifier.com/

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
There is a super secure vault protected by a powerful security mechanism, that holds priceless jewels, only accessible to those wit a valid. However, the code for validating keys has been leaked and you need to be fastest at finding a valid key!!! Here is the leaked code:
```javascript
(function(p) {
    p[0].parseValue = p[1][4](p[1]);
}([
    window, [
        function(s) {
            return s[0] = s[1].join(''),
                typeof eval(s[0]) === 'function' && s[0] === 'alert';
        },
        function(H) {
            for (H[1] = 0, H[2] = H[0].length; H[1] < H[2]; ++H[1]) {
                H[0][H[1]] = String.fromCharCode(H[0][H[1]].charCodeAt(0) + 1);
            }
        },
        function(g) {
            return String.fromCharCode(g[0]) === g[1];
        },
        function(U) {
            while (U[1] != 0) {
                U[2] = U[0] & U[1], U[0] = U[0] ^ U[1], U[1] = U[2] << 1;
            }
            return U[0];
        },
        function(l) {
            return function(j) {
                var m;
                return m = [], j.match(/([2-70-18-9]){1,}([^6-90-5])/g).forEach(l[5]([l, m])), l[1]([m]), l[0]([j, m]);
            };
        },
        function(A) {
            return function(y) {
                var r = [0];
                var len = y.length - 1;
                var i = 0;
                for (; i < len; ++i) {
                    r[0] = A[0][3]([r[0], y[i]]);
                }
                A[0][2]([r[0], y[i]]) && A[1].push(y[i]);
            };
        }
    ]
]));
```
### Rationale 

# 4 - Antidotes for the Trip
### Problem
```javascript
'use strict';

// <OUR CODE GOES HERE 1>

var messenger = new Messenger({
    from: '',
    to: '',
    text: ''
});

// <OUR CODE GOES HERE 2>

var timer = setInterval(function() {
    nextLocation();
    if (location === CAPITAL) {
        var message = messenger.deliverMessage();

        // <OUR CODE GOES HERE 3>

        clearInterval(timer);
    }

}, 1000);
```


### Rationale 

<p align="center"><img src="https://i.imgur.com/rOHYG2L.png"></p>
