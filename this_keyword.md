# this가 가리키는 것

- 본인을 담고 있는 오브젝트를 표현
  - 함수를 포함하고 있는 오브젝트 (오브젝트 내 함수(메서드)에서 사용, arrow function 일 시 함수밖에 있는 this를 그대로 사용)
  - window (전역에 선언, 함수 안에 선언(strict 모드에선 undefined)) -> 함수나 변수를 전역 공간에 만들면 window(global object)에서 보관하기 때문
- constructor에서의 this 키워드
  - 새로 생성되는 object(instance)를 의미
- addEventListener의 function에서 사용 시
  - e.currentTarget ( e.currentTarget는? 이벤트가 동작하고 있는 HTML 요소 )

## 본인을 담고 있는 오브젝트를 표현

```javascript
    let data = {
        testArray : [1,2,3],
        testFunc : function() {
            console.log(this); // data 오브젝트를 출력
            data.testArray.forEach(function() {
                console.log(this) // 오브젝트에서 선언된 this가 아니므로 window 출력
            })
        }
    }
    data.testFunc();
```

```
Object
Window
Window
Window
```

### arrow function 일 시?

```javascript
    let dataOfArrowFunc = {
        testArrayOfArrowFunc : [1,2,3],
        testFuncOfArrowFunc : function() {
            console.log(this); // dataOfArrowFunc 오브젝트를 출력
            dataOfArrowFunc.testArrayOfArrowFunc.forEach(() => {
                console.log(this) // 상위의 this를 가져오므로 window가 아닌 dataOfArrowFunc 오브젝트를 출력
            })
        }
    }
    dataOfArrowFunc.testFuncOfArrowFunc();
```



```
Object
Object
Object
Object
```

## constructor에서의 this 키워드

```javascript
    function TestConstructor(name) {
        this.name = name;
        this.testFunction = function () {
            console.log(this); // 인스턴스 출력
            console.log(this.name); // 인스턴스의 name 출력
        }
    }

    const testInstance = new TestConstructor('testName');
    testInstance.testFunction();
```

```
TestConstructor
testName
```

## addEventListener의 function에서 사용 시

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <butto id="test">test button</butto>
</body>
</html>
<script>
    document.getElementById('test').addEventListener('click',
        function(event) {
            console.log(this); // event.currentTarget 출력
            console.log(event.currentTarget === this); // true 출력
            let testArray = [1,2,3];
            testArray.forEach(function() {
                console.log(this); // addEventListener에서 선언된 this가 아니므로 window 출력
            });
        })
</script>
```

```
<butto id="test">test button</butto>
true
Window
Window
Window
```

### event.currnetTarget vs event.target

> `event.currnetTarget`는 이벤트 핸들러가 부착되어 있는 요소, `event.target`은 클라이언트가 클릭한 요소 (자식 포함)
