## 서론
**! 이 글은 promise 사용을 권장하고자 흥미 유발을 위해 작성된 글입니다.**

javascript promise는 promise(약속)이라는 개념으로 동기와 비동기 처리를 깔끔하게 이을 수 있게하고, callback을 통해서 무작정 모든게 잘 되길 기대하는 코드를 작성하지 않을 수 있도록 해줍니다. 

promise는 ES6 스펙이고, 현재 외부 라이브러리를 통해 ES5를 지원하는 대부분의 브라우저에서도 지원되고 있습니다.

## promise가 없던 시절에
**CALLBACK OF HELL**

우리는 2 depth만 들어가도 혼이 빠져나갈듯한 callback 코드를 작성해야만 했습니다. 왜냐하면 callback은 작성하기 쉽고, 개념적으로 접근이 쉽기 때문입니다. 그리고 당시에 혁신적인 jQuery를 이용한 UI개발 덕분에도 callback은 그야말로 신세계였기 때문입니다.

callback은 1 depth로만 작성하면 참 좋습니다. 사용자가 넣어준 callback들이 잘 호출되기를 기대하면서 간단히 1 depth만 chained-scope를 둘러보기만 하면 디버깅도 쉬우니까요. 하지만 1 depth callback만으로는 모든 처리를 표현하기엔 불충분하고, 그래서 아래와 같이 최소 2 depth callback을 사용할 수 밖에 없습니다.
```javascript
//주의 : 아래 코드를 보고 암에 걸릴 수도 있습니다!

function downloadFile(url, onLoad, onError){
	//download file using XHR
	var xhr = new XMLHttpRequest();
    
    xhr.onload = onLoad;
    xhr.onerror = onError;
    
    xhr.open('GET', url, true);
    xhr.send();
}

function onLoad(data){
	//do something
    handleData(data, onHandleDataSucess, onHandleDataFailure);
}
function handleData(data, success, failure){
	var handler = new MyHandler();
    handler.onsuccess = success;
    handler.onfailure = failure;
    
    handler.handle();
}
function onHandleDataSuccess(result){
	//do something
}
function onHandleDataFailure(reason){
	//do something
}

function onError(e){
	//do something
}

downloadFile(url, onLoad, onError);
```

겨우 2depth인데 벌써 숨이 턱턱 막히는 기분이 듭니다. `//do something` 주석 대신  코드가 10줄 씩만 들어가도 이 코드블럭 자체를 삭제해버리고 싶은 충동이 들겁니다.

위의 문제를 해결하기 위해 같은 `연속적인 비동기/동기처리`의 형태를 코드의 순서를 보장하며 수행해주는 `waterfall`이라는 개념이 도입되어 아래와 같이 나름 깔끔하게 코드를 작성할 수 있게 되었습니다.
```javascript
waterfall( [first, second, third] );

function first(params, success, failure){}
function second(params,success, failure){}
function third(params,success, failure){}
```

하지만 여전히 부족합니다. first, second, third가 모두 같은 시그니쳐를 가져야한다는 것부터 벌써 마음에 들지 않습니다.

## promise라는 개념을 적용하면
**! 완전 신세계**

간단히 위에 나왔던 `암 유발 코드블럭`을 promise를 사용해 표현해보겠습니다.
```javascript
function downloadFile(url){
	//download file using XHR
	var xhr = new XMLHttpRequest(),
    	promise = new Promise();
    
    xhr.onload = function(response){ 
        promise.resolve(response);
    };
    xhr.onerror = function(e){
    	promise.reject(e);
    };
    
    xhr.open('GET', url, true);
    xhr.send();
    
    return promise;
}

function handleData(data){
	var handler = new MyHandler(),
    	promise = new Promise();
    
    handler.onsuccess = function(data){ 
        promise.resolve(data);
    };
    handler.onfailure = function(e){
    	promise.reject(e);
    };
    
    handler.handle();
    
    return promise;
}

downloadFile(url)
	.then(function(response){
    	//do something
    	return handledata(response.myData);
    }, function(e){
    	//do something
        return 'Error ' + e.target.status + ' occurred!';
    })
    .then(function(result){
    	//do something
    }, function(reason){
    	//do something
    	alert(reason);
    });
```

앞에서 봤던 `암유발 코드블럭`에 있던 callback들이 downloadFile이 리턴한 promise의 then function의 inline callback으로 붙게되었습니다. 물론 `암유발 코드블럭`에 있는 callback들도 inline으로 사용할 수 있지만, 그렇게되면 `암유발 코드블럭`이 `더욱 향상된 암유발 코드블럭`으로 변하게 됩니다.

**promise를 적용하게되면 예제처럼 동기/비동기처리 주체는 promise만 return하면 책임이 끝나게 됩니다.
그리고 promise 성공 실패에 대한 callback 처리는 promise 객체를 외부로 던짐으로써 동기/비동기처리 주체가 신경쓰지 않아도 되게 되는거죠.**
바로 이게 핵심입니다.

## 간단히 promise 개념에 대해 설명하자면
promise는 약속을 표현하는 객체입니다. 약속은 `~을 기대한다`라고 표현될 수 있고, 그래서 아직 처리되지 않을 수 있다는 전제가 깔려있습니다. 처리가 완료(resolve)되었을 시 추가적인 행위를 할 수 있고, 반대로 처리가 반려(reject)되었을 시에도 추가적인 행위를 할 수 있습니다.

일상 생활의 약속을 promise 객체로 표현해보겠습니다.
예는 다음과 같습니다.
```
A는 친구인 B에게 밥을 사주기로 약속을 했습니다.
B는 A가 밥을 사주면 A에게 선물을 하기로 결심합니다.
A는 정말로 B에게 밥을 사주었고, 그래서 B는 A에게 선물을 줍니다.
```

```javascript
var promiseToServeDinner = new Promise(),
	A = { stuffs : [] },
	B = { stuffs : [] };
    
function serveDinner(to){
	//do something
    
    return promiseToServeDinner.resolve({
    	server : this,
        to : to
    });
}

function givePresent(to, present){
    to.stuffs.push(present);
}

//A serves dinner to B
serveDinner.apply(A, [B])
	.then(function(result){
    	if(result.to !== B) return;
        
    	//B gives a present to A
		givePresent.apply(B, [A, B.stuffs.pop()]);
	});
```

## promise에 대한 기술적인 설명
흥미 유발이 되었으면 이제 promise에 대한 [MDN 공식 문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)를 참고할 때입니다!

## promise를 잘못 사용하게 되면
**PROMISE OF HELL**

promise가 없던 시절과 다를 바 없습니다.
promise를 모르는 사람들에게는 이 코드가 정말 의도된 코드인지 아닌지 알 수가 없습니다.

아래는 promise를 이해하지 않고 사용할 때 생길 수 있는 문제들입니다.
**1. promise를 callback처럼 중첩해서 사용한다.**
```javascript
promise1.then(function(){
    prosmie2.then(function(){
        promise3.then(function(){
            //STOP!!!!!
        });
    });
});
```
**2. resolve case와 reject case를 분리해서 사용하지 않는다.**
```javascript
var promiseAlwaysResolve = new Promise(),
	simplePromise = new Promise();
    
simplePromise
    .then(function(result){
        return promiseAlwaysResolve.resolve(result);
    }, function(reason){
        return promiseAlwaysResolve.resolve(reason);
    }).then(function(result){
        if(result === 'success'){
        	//do something
            //STOP!!!!
        } else if(result === 'failure'){
        	//do something
            //STOP!!!!
        }
    }, function(){
        // where's my promise?!
    });
```

**3. 추가적으로 then이 붙을 수 있는데도 현재 작업중인 then에서 아무것도 return하지 않는다.**
```javascript
var simplePromise = new Promise();
    
simplePromise
    .then(function(result){
        return result.myData;
    }, function(reason){
        console.log(reason);
    }).then(function(result){
    	//do something
    }, function(reason){
    	// where's my reason?!
    });
```

## promise는 참 좋습니다. 다 좋은데,
1. js를 접한지 얼마 안된 사용자에게는 어려울 수 있는 개념입니다. 충분히 공부를 하고 사용할 필요가 있습니다.
2. 현재 우리가 사용하는 브라우저는 대부분 ES5를 지원합니다. 그래서 사용하려면 추가적으로 라이브러리를 사용해야 합니다. 

## 결론
promise를 사용해서 사용자가 비동기/동기처리가 자연스럽게 연결되고 구겨 넣어지지 않은 코드를 작성할 수 있었으면 좋겠습니다.
