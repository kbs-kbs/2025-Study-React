# 2025-Study-React


## 리액트 설치
CDN(Content Delivery Network)을 통한 라이브러리 설치

```html
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
```

## 리액트와 바닐라 JS 비교

```html title:vanilla-js.html
<!DOCTYPE html>
<html>
<body>
    <div>
        <span>Total clicks: 0</span>
        <button>Click me</button>
    </div>
</body>
<script>
    let count = 0;
    const p = document.querySelector('body > div > p');
    const button = document.querySelector('body > div > button');
    
    button.addEventListener("click", handleClick);
    
    function handleClick() {
        counter++;
        p.innerText = `Total clicks: ${count}`;
    }
</script>
</html>
```

```html react.html
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script type="module">
    import React from 'https://esm.sh/react@19/?dev';
    import ReactDOMClient from 'https://esm.sh/react-dom@19/client?dev';

    const root = ReactDOMClient.createRoot(document.getElementById('root'));

    const Counter = () => {
        const [count, setCount] = React.useState(0);

        function handleClick() {
            setCount(count + 1); // 렌더링 코드 필요 없음
        }

        return React.createElement('div', null, [
            React.createElement('p', null, `Total clicks: ${count}`),
            React.createElement('button', {
                onClick: handleClick
            }, 'Click me')
        ]);
    };

    const DoubleCounter = React.createElement('div', null, [
        React.createElement(Counter),
        React.createElement(Counter)
    ]);

    root.render(DoubleCounter);
</script>
</html>
```

> [!Warning]
> Warning: createRoot(): Creating roots directly with document.body is discouraged, since its children are often manipulated by third-party scripts and browser extensions. This may lead to subtle reconciliation issues. Try using a container element created for your app.
>    
> **해결 방법**:
> - `document.body` 대신 별도의 컨테이너 요소를 만들어서 React 앱을 렌더링하세요.
> - React는 `id="root"`와 같은 컨테이너를 사용하는 것을 표준으로 삼고 있습니다.



> [!Note]
> **state의 역할**:      
> 지역변수   
> **setState의 역할**:    
> - state의 값을 변경하는 클로저 + 컴포넌트 리렌더링    

> [!Note]
> setState(state + 1)과 setState((state) => state + 1)의 차이     
> 이벤트에서 setState(state + 1)을 3번 실행하면 결과는 (처음 state) + 1 이 됨     
> 반면, setState((state) => state + 1)을 3번 실행하면 (처음 state) + 3이 됨    

```html title:react-without-react-dom.html
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script type="module">
    import React from 'https://esm.sh/react@19/?dev';

    const root = document.getElementById('root');
    const Counter = (() => {
        let count = 0;
        return () => {
            function handleClick() {
                count++;
                render(Counter(), root);
            }

            return React.createElement('div', null, [
                React.createElement('p', null, `Total clicks: ${count}`),
                React.createElement('button', {
                    onClick: handleClick
                }, 'Click me')
            ]);
        };
    })(); // 즉시 실행 함수(IIFE)

    render(Counter(), root);

    function render(element, parent) {
        const domElement = document.createElement(element.type);
        const children = element.props.children;

        if (parent == root) {
            root.textContent = '';
        }

        if ('onClick' in element.props) {
            domElement.addEventListener('click', element.props.onClick);
        } // 예시로 'onClick'만 체크

        if (typeof children === 'string') {
            domElement.textContent = children;
        } else {
            React.Children.forEach(children, child => {
                render(child, domElement);
            });
        }

        parent.appendChild(domElement);
    }
</script>
</html>
```



```html title:react-double-counter.html
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script type="module">
    import React from 'https://esm.sh/react@19/?dev';
    import ReactDOMClient from 'https://esm.sh/react-dom@19/client?dev';

    const root = ReactDOMClient.createRoot(document.getElementById('root'));

    const Counter = () => {
        const [count, setCount] = React.useState(0);

        function handleClick() {
            setCount((count) => count + 1); // 렌더링 코드 필요 없음
        }

        return React.createElement('div', null, [
            React.createElement('p', null, `Total clicks: ${count}`),
            React.createElement('button', {
                onClick: handleClick
            }, 'Click me')
        ]);
    };

    const DoubleCounter = React.createElement('div', null, [
        React.createElement(Counter),
        React.createElement(Counter)
    ]);

    root.render(DoubleCounter);
</script>
</html>
```
