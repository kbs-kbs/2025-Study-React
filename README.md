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
</body>
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script>
    let count = 0;

    render();

    function render() {
        const P = React.createElement("p", null, `Total clicks: ${count}`);
        const Button = React.createElement("button", {
            onClick: handleClick
        }, "Click me");
        const Container = React.createElement("div", null, [P, Button]);

        ReactDOM.createRoot(document.body).render(Container);
    }

    function handleClick() {
        count++;
        render();
    }
</script>
</html>
```

```html title:react-without-react-dom.html
<!DOCTYPE html>
<html>
<body>
</body>
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script>
    let count = 0;
    const P = React.createElement("p", null, "Total clicks: 0");
    const Button = React.createElement("button", {
        onClick: handleClick
    }, "Click me");
    const Container = React.createElement("div", null, [P, Button]);

    render(Container, document.body);

    function render(element, parent) {
        const domElement = document.createElement(element.type);
        if ('onClick' in element.props) {
            domElement.addEventListener("click", element.props.onClick);
        }
        const children = element.props.children
        if (typeof children === "string") {
            domElement.textContent = children;
        } else {
            React.Children.forEach(children, child => {
                renderer(child, domElement);
            })
        }
        parent.appendChild(domElement);
    }

    function handleClick() {
        count++;
        document.querySelector('body > div > p').innerText = `Total clicks: ${count}`;
    }
</script>
</html>
```
