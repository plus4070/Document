```javascript
function setTemplate(template) {
  var regex = /\$\{(\w+)\}/g;

  return function(model) {
    return template.replace(regex, function(match, p1) {
      return model[p1] || '';
    });
  };
};

```

### 사용

``` javascript
var boardTemplate = setTemplate(jQuery("#board-template").text());
jQuery.each(boardList,  function(idx, board){
jQuery(".main-service-list").append((boardTemplate(boardList[idx])));
```
