I just recently spent a while learning the mechanics of key events in javascript. There are many incompatibilities between each of the various browsers, which makes the task somewhat difficult.

### Getting the Key
The first problem I encountered was how to get the key from the event. Internet Explorer uses `event.keyCode` to store the numeric representation of the key. Mozilla historically used `event.which`, but now uses either `event.keyCode` and `event.charCode` depending on the circumstance. However, `event.which` is still supported and always contains the key code, whereas only one of either `event.keyCode` or `event.charCode` contain the key code. Because of these oddities, it is necessary to use some logic to figure out exactly what has the key we want. For that we do the following:

~~~js

    var key = event.which || event.keyCode;

~~~

We can check if `event.which` exists. If it does we are on Mozilla and will get the `event.which` property to get the key code. If it doesn't exist then that means we are on Internet Explorer and we need to check the `event.keyCode` property.

### The Difference Between Key Events
Keyup and keydown are rather different from keypress. Both keyup and keydown's key codes map to the specific keyboard that the user is using. Meaning there is no consistent way to determine the key between any two computers. However, the keypress event behaves differently. Instead of storing the numeric representation of the keyboard key, it stores the UTF-8 encoding of the character which is easy to determine across different systems. The lesson here is that if you want to know if a key was pushed or released, but don't care what it is, you can use keydown and keyup. When you must know the key value that is being typed using keypress is the only reliable option.

### The Shift Key
All browsers have support to figure out if the shift key is being pressed for each of the event types. The problem is that there is no way to tell what the original unshifted key is, because the event will only tell you the shifted key code. For letters, it is simple to convert back to lowercase with String's `toLowerCase()` method, however, for the number and symbol keys, the only way I could think of is to have a translation table. As a result I came up with this:

~~~js

    var deshift = {'~': '`', '!': '1', '@': '2', '#': '3', '$': '4', '%': '5', '^': '6',
                   '&': '7', '*': '8', '(': '9', ')': '0', '_': '-', '+': '=', '{':'[',
                   '}':']', '|': '\\', ':': ';', '"': '\'', '<': ',', '>': '.', '?': '/'};

~~~

However, I'm using an American standard keyboard. If another keyboard setup has other keys that are modified by the shift key, this will not solve the problem.

I discovered all of this while writing an extension to the [Prototype library][1] called [Blacksmith][2].

[1]: http://www.prototypejs.org
[2]: https://github.com/blatyo/blacksmith
