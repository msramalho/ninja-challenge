<p align="center"><img src="https://i.imgur.com/cSV2br2.png"></p>

<h3 align="center">This repo contains some thoughts by <i>maps</i> aka <i><a href="https://github.com/msramalho">msramalho</a></i> on the problems of the 
<a href="https://ninjachallenge.jscrambler.com/">ninja challenge 2018</a>, by 
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
After some testing and simplification I came to understand that this function is essentially a **fibonacci sequence** where the 40th element had an increment of 100 and this change propagated for the consequent numbers. The function also had a few peculiarities for return values of other values (negatives, non-numeric, ...). 

#### Initial [dumb] approach
 0. (You only need to submit the body of the function and the header given is: `function sequence(max){...}`)
 1. Notice that weird characters are representing names of variables (`ˠビ`) and names of functions (`ʹϡ`).  
 2. Replace all occurrences of each of these patterns with friendly characters, like letters from `a` to `q` and work from there... **❗this is not so simple❗** if you analyse the code properly, you'll notice that, in certain points, the `length` property is used on functions (the number of bytes of that function when converted to text), which means that if `ʹϡ` is replaced by `a` then 3 bytes are lost and this leads to a function that does something else!! However, if you replace carefully (respecting byte length) this will produce a more readable piece of code. (The same type of thing would happen if you tried to replace the `;;` by `;`)
 3. Next steps are essentially simplifying, here are some of the more obvious things:
    * replace function `length`s by their value
    * remove unused functions
    * keep on removing variables
    * simplify operators (a lot can be done on this topic): `===` for `==` where possible, `a+=b` instead of `a = a + b`, ...
 4. You will **eventually** be able to remove all functions from the code and end up with something like (of course all unnecessary whitespace can also be removed):
```javascript
var n = 0,
    e = 0,
    f = 1,
    o = !!max;
for (; e < max;) o = 40 == ++e ? 100 + n + f : n + f, n = f, f = o;
return o || 0
```
 5. At this stage I might have tried to use something like a [js minifier](https://javascript-minifier.com/) and learn some new tactics like declaring the variables inside the loop, something like [**79 bytes**]:
```javascript
for(var a=0,r=0,n=1,f=!!max;r<max;)f=40==(r+=1)?100+a+n:a+n,a=n,n=f;return f||0
```
 6. Now a _silver lining_: if you can understand and apply [JS operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) you can start doing simultaneous updates of the variables inside the loop, instead of comma separated expressions, here is an intermediary example [**74 bytes**]:
```javascript
for(var n=0,e=0,f=1,o=!!max;e<max;)o=n+(n=f)+100*(40==++e),f=o;return o||0
```
 7. I also did some attempts at this simultaneous assignment using lists, but this approach was to costly in terms of bytes [**76 bytes**]:
```javascript
for(var n=0,e=0,f=1,o=!!max;e<max;)n=[f,o=f+=n+100*(40==++e)][0];return o||0
```

 8. Next, there was more simultaneous assignment simplification to be done [**67 bytes**]:
```javascript
for(var n=0,e=0,f=1,o=max;e<max;f=o=n+(n=f)+100*(40==++e));return o
```
 9. At this stage, there was still an unnecessary variable [**62 bytes**]:
```javascript
for(var n=0,e=0,f=1;e<max;f=n+(n=f)+100*(40==++e));return e&&f
```
 10. At this point there seemed little hope of improval, but a counter intuitive approach actually worked: reverting the cycle so that we remove the counter variable and decrease the value of `max` instead, like so [**62 bytes**] as well (we need to compare to `39!=102334155` instead of `40`), but...:
```javascript
for(var a=0,d=1;max--;d+=a+100*((a=d)==102334155));return a&&d
```
 11. At this point, I dare the reader to think of approaches, because it was not that obvious... 
 11. The click happened when an alternative to `((a=d)==102334155)` was actually to use the `%` module operator, given that `((a=d)%1000==155)` was true **and** there was no collision in the tests... [**61 bytes**]
```javascript
for(var a=0,d=1;max--;d+=a+100*((a=d)%1000==155));return a&&d
```
 10. Also, in JS `1000 == 1e3`, so another byte...
 11. But then... what if the module operator could be taken further... this required a `for` loop to look for `x` such that `(a=d)%x==y` in order to make `y` have a single digit (`y<10`)... and **this was possible**, for example for `x=61,y=6` [**57 bytes**] (notice that `x` and `y` had to be such that there was no collision in the previous factorials... this also took some time to test!!):
```javascript
for(var a=0,d=1;max--;d+=a+100*((a=d)%61==6));return a&&d
```
 10. You can't go any further, right? wrong, always wrong... What if there was an `x` so that `(a=d)%x==0`? that would be the same amount of bytes as `x=61,y=6`... yes, but if `y=0`, we could still go further... Meet magic number **77**!! [**55 bytes**]:
```javascript
for(var a=0,d=1;max--;d+=a+100*!((a=d)%77));return a&&d
```
 10. This is as far as I went: **55 f\*\*king bytes**. Nevertheless, someone was able to take it further (I estimate by one byte), but you may still do better so any PR is welcome and kudos will be widespread if you can improve this answer(I suspect a different approach would be needed)!!


#### _Post-mortem_ approach
 1. Don't waste time, simple copy and paste the orignal code into a [minifier](https://javascript-minifier.com/) which would output [**273 bytes**] (without using `max` - that must be done!) not a bad start :
```javascript
var t=0,u=t,r="",f=10,o=u+1,c="length",i=n&&u+f;function e(){t=o}function a(){o=i}function g(){return t+o}function h(n){u+=l(e,r)[c]-l(r,a)[c],i=u===f*n?l(f*f,g()):g(),e(),a()}function l(n,t,u){return u?n||t:t+n}for(;u<n;)h(c[c]-l(r,f)[c]);return l(i,l(a,r)[c]-l(r,a)[c],l)
```
 2. This would save steps 1. and 2., which is a lot!! The other steps are essentially the same.

#### A funny view of my progress:
```javascript
var r=0,t=0,u=1,e=!!max;function f(){return r+u}for(;t<max;)e=40==(t+=1)?100+f():f(),r=u,u=e;return e||0
var n=0,e=0,f=1,o=!!max;for(;e<max;)o=40==(e+=1)?100+n+f:n+f,n=f,f=o;return o||0
var n=0,e=0,f=1,o=!!max;for(;e<max;)o=40==++e?100+n+f:n+f,n=f,f=o;return o||0
for(var n=0,e=0,f=1,o=!!max;e<max;)o=f+n+100*(40==++e),n=[f,f=o][0];return o||0
for(var n=0,e=0,f=1,o=!!max;e<max;)n=[f,o=f+=n+100*(40==++e)][0];return o||0
for(var n=0,e=0,f=1,o=!!max;e<max;)o=n+f+100*(40==++e),n=f,f=o;return o||0
for(var n=0,e=0,f=1,o=!!max;e<max;)o=n+(n=f)+100*(40==++e),f=o;return o||0
for(var n=0,e=0,f=1,o=!!max;e<max;)f=o=n+(n=f)+100*(40==++e);return o||0
for(var n=0,e=0,f=1,o=!!max;e<max;)f=o=n+(n=f)+100*(40==++e);return +o
for(var n=0,e=0,f=1,o=+max;e<max;)f=o=n+(n=f)+100*(40==++e);return +o
for(var n=0,e=0,f=1,o=+max;e<max;)f=o=n+(n=f)+100*(40==++e);return o
for(var n=0,e=0,f=1,o=max;e<max;)f=o=n+(n=f)+100*(40==++e);return o
for(var n=0,e=0,f=1,o=max;e<max;f=o=n+(n=f)+100*(40==++e));return o
for(var n=0,e=0,f=1;e<max;f=n+(n=f)+100*(40==++e));return e&&f
for(var a=0,d=1;max--;d+=a+100*((a=d)==102334155));return a&&d
for(var a=0,d=1;max--;d+=a+100*((a=d)%1000==155));return a&&d
for(var a=0,d=1;max--;d+=a+100*((a=d)%1e3==155));return a&&d
for(var a=0,d=1;max--;d=a+100*(a%1e2==86)+(a=d));return a&&d
for(var a=0,d=1;max--;d=a+100*(a%86==38)+(a=d));return a&&d
for(var a=0,d=1;max--;d=a+100*(a%35==6)+(a=d));return a&&d
for(var a=0,d=1;max--;d+=a+100*((a=d)%61==6));return a&&d
for(var a=0,d=1;max--;d+=a+100*((a=d)%77==0));return a&&d
for(var a=0,d=1;max--;d+=a+100*!((a=d)%77));return a&&d
```

#### Official solution [55 bytes]
```javascript
for(var a=1,g=0;max--;)g+=a+!((a=g||1)%77)*100;return g
```

# 2 - Don’t mess with my Shuriken
### Problem
Replace `<OUR CODE GOES HERE>` by a condition that allows the ninja to detect that there was some sabotage. The ninja will perform 5 weapon attacks, reload, and attack 5 times once more, so as to kill both enemies (`DrunkenFist` and `FlyingPunch`) after 10 throws. I advise you to look at the code and to understand what is going on. 
```javascript
'use strict';

var EnemyNinja = function(name) {
    return (function() {
        var _health = 100;
        var _name = name;

        return Object.freeze({ // methods and properties of this object cannot be changed
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
My first few approaches were a bit overcomplicated, I was trying to assert multiple conditions at once, but given the script and the assumption that the ninja would alwyas attack 5 times (`5 * 20 == 100`) recharge for 100 times (one energy point per loop) and then attack 5 more, things got easier.
#### Overcomplicated approach
I need a counter to make sure that the number of times the functions is called match the value of `_energy` the ninja weapon should have. 

But we could only write code inside the condition, there is no way to do a `var x = 0;` in there... nor would we be able to keep the value of that variable over the next iterations...

Remember, everything in JS is an object, and so are functions! What I did was add a property to the `fn` function so that it could be incremented on every call of itself. How to do this? checking for its precious instantiation with `typeof` and ternary operators, incrementing if it is defined and setting to 0 if it is not:
```javascript
fn.called = (typeof fn.called === "undefined") ? 0 : fn.called + 1
```
So the only thing to do now would be to compare it to the expected value of energy:
```javascript
if (
    (fn.called = (typeof fn.called === "undefined") ? 0 : fn.called + 1) != _energy
) {
    throw 'Weapon-tampering-detected-o-jutsu!';
}
```
Thre you go, it works! 
#### Why-did-I-not-think-of-this-before approach
However, there was another approach I was latter told to work, if I recall correctly it was...
```javascript
_energy < 0
```
Yes, just that... this was partly due to the simplicity of the so-called tampering they performed... Many other conditions could be used, but it was just a matter of trying some simpler things before overdoing it. 

#### Official Solution
```javascript
fn !== this.recharge
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
            return s[0] = s[1].join(''), typeof eval(s[0]) === 'function' && s[0] === 'alert';
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
This was designed to make you despair over what function calling what function with which function returned whoose function, function. Although a good look at the code was essential to understant that it is setting the following function `window.parseValue` and that that should be the function to call on your tests to check for a valid key, the best approach here would be debugging! So using a debugger or just some console logs (my approach)... 

I inserted a `console.log("this is function XXX being called with argumetns YYY");` in every function. What I first understood was that the key should be, in some way compatile with the regex `/([2-70-18-9]){1,}([^6-90-5])/g` and so I started using some patterns that matched it like `341a`. At this stage the code would go through at stop at another point, I then realized that the pattern should be so that the sum of each digit in the key (in `341a` it is `8`) **plus 1** should match the [charCode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt) value of the last char of the key. At this stage I used a key like `9999999999997s` meaning that the sum of the digits in `9999999999997` would match an `r` and so the last char should be `s`.

At this point, the program flow would continue and reach the `eval` on the first function of the array, but it crashed inside it. After inspecting what happened to the string I supplied and the `eval` called, I understood that the the key should be such that foreach instance of the pattern in it, the last chars of each instance would be concatenated and the `eval` would be called. There was also this:
```javascript
typeof eval(s[0]) === 'function' && s[0] === 'alert';
```
which led me to conclude that the chars should match the word `alert` and that would evaluate to the JS `alert("something");` function. After constructing it, I got something like:

```99999999996`999999999998k999999999991d9999999999995q9999999999997s```

This would spell `alert` and the vault was open!! Other combinations of numbers and spaces could be used, but every regex match must lead to the addition of another letter in `alert`!

# 4 - Antidotes for the Trip
### Problem
You need to equip your messenger with the skills necessary to deliver a Message to the Empero:
```
From: Ninja
To: Emperor

    Highness,
         My spying mission was a success and I was able to gather valuable information. 
         The rival clan will attack the capital tomorrow at 9PM.
    Ninja
```
However, many messengers had died on their way due to poisoning. And, in some cities, poison is also known as **monkey patching**. The could would be poisoned in several undisclosed places and we needed to build the antidotes for the messenger trip. This was the code we saw:
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
A lot to be said on this one, I will not reveal the whole solution, just give some tips and remarks on what I tried and latter learned:
 1. The first `<OUR CODE GOES HERE 1>` was the simplest, you needed only to create an object-returning function (aka class): 
 ```javascript
 function Messenger(info) {
    this.from = info.from;
    this.to = info.to;
    this.text = info.text;
}
```
This would display the next hint: `You never left the castle`...
 2. Many things were tried but I will only give an example. By defining my on `setInterval` function I would have access to the poisoned code and I was able to do a lot of inspection to it using `if`s and `while(true);`s (if the code reached an infinite-loop it would take significantly longer in the server so we could due this kind of blind inspection). 
 3. As a matter of fact, I managed to modified their poisoned version and run a valid `eval` on it, successfully entering the `if` and running the third 
 
 An example for the code in `<OUR CODE GOES HERE 2>` that did this, would be:
 ```javascript
 window.setInterval = function(f, time){
    var s = ""+f; // convert to string
    s = "var f = " + s; // cast to f variable
    s = s.replace("===", "!="); // override their code
    eval(s); // eval it
    f(); // call f and get inside <OUR CODE GOES HERE 3>
    return 1;
}
```
I knew I had gotten in, because my `<OUR CODE GOES HERE 3>` was `while(true);` 😏

3. The error you got when doing this reverse-monkey-patching was `You tried to freeze but it is always sunny`. This was a strong enough hint, but I was too locked inside my head and could not make sense out of it (one needs to learn how to think). You needed to use some machiavelic stuff to be able to overcome it, and that is the mind challenge I leave you with... Of course I tried dozens of other things even before I got here, but these problems, in the end, just need a clear approach in which you don't sidetrack. However, if in hindsight it looks easy, let me tell you it is not! at all!

#### Official solution
```javascript
   // Part 1
   function Messenger (message) {
     this.message = message;
     this.secretMessage = message.text;
   }
    
   Messenger.prototype.deliverMessage = function () {
     return this.message;
   };
    
   delete window.setInterval;
    
   // Part 2
   delete Object.freeze;
   var iframe = document.body.appendChild(document.createElement('frame'));
   var newWindow = iframe.contentWindow
   Object.prototype.freeze = newWindow.Object.prototype.freeze;
    
   // Part 3
   message.text = messenger.secretMessage;
```

<h2 align="center">The End </h2>

<p align="center"><img src="https://i.imgur.com/rOHYG2L.png"></p>
