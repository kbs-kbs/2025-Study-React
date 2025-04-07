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
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script>
    let count = 0;
    const P = React.createElement('p', null, `Total clicks: ${count}`);
    const Button = React.createElement('button', {
        onClick: handleClick
    }, 'Click me');
    const Container = React.createElement('div', null, [P, Button]);
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(Container);

    function handleClick() {
        count++;
        P.render(Container);
    }
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

```html title:react-without-react-dom.html
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script>
    const root = document.getElementById('root');
    let count = 0;
    const Counter = () => {
        const P = React.createElement('p', null, `Total clicks: ${count}`);
        const Button = React.createElement('button', {
            onClick: handleClick
        }, 'Click me');
        const Container = React.createElement('div', null, [P, Button]);

        function handleClick() {
            count++;
            render(Counter(), root);
        }

        return Container
    };

    render(Counter(), root);

    function render(element, parent) {
        root.textContent = '';
        const domElement = document.createElement(element.type);

        if ('onClick' in element.props) {
            domElement.addEventListener('click', element.props.onClick);
        }

        const children = element.props.children
        if (typeof children === 'string') {
            domElement.textContent = children;
        } else {
            React.Children.forEach(children, child => {
                render(child, domElement);
            })
        }
        parent.appendChild(domElement);
    }
</script>
</html>
```

```html title:react.html
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
<script>
    const { useState } = React;
    const root = ReactDOM.createRoot(document.getElementById('root'));
    const Counter = () => {
        const [count, setCount] = useState(0);
        const P = React.createElement('p', null, `Total clicks: ${count}`);
        const Button = React.createElement('button', {
            onClick: handleClick
        }, 'Click me');
        const Container = React.createElement('div', null, [P, Button]);

        function handleClick() {
            setCount(count + 1);
        }

        return Container
    };

    root.render(React.createElement(Counter));
</script>
</html>
```
