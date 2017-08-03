```
var name = 'World!';
(function() {
  if (typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbye' + name);
  } else {
    console.log('Hello' + name);
  }
})();

(function() {
  if (typeof name === 'undefined') {
    console.log('Goodbye' + name);
  } else {
    var name = 'Jack';
    console.log('Hello' + name);
  }
})();

```