```
function hasSpecialWord(word) {
    var reg = /[?~!@#$%^&*\()\-=+_'"`<>|\\]/gi; 
    if(reg.test(word)){
      return false;
    }
    return true;
  }
```
