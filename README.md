# Quick knowledge React
React é uma biblioteca pra construção de interfaces usando javascript como base.

## Referência
* [cloneElement()](#cloneelement)
* [createElement()](#createelement)
* [isValidElement()](#isvalidelement)
* [React.Children](#reactchildren)
  * [React.Children.count](#reactchildrencount)
  * [React.Children.forEach](#reactchildrenforeach)
  * [React.Children.map](#reactchildrenmap)
  * [React.Children.only](#reactchildrenonly)
  * [React.Children.toArray](#reactchildrentoarray)
* [React.Component](#reactcomponent)
  * [componentDidCatch()](#componentdidcatch)
  * [componentDidMount()](#componentdidmount)
  * [componentDidUpdate()](#componentdidupdate)
  * [componentWillMount()  ](#componentwillmount)
  * [componentWillReceiveProps()  ](#componentwillreceiveprops)
  * [componentWillUnmount()](#componentwillunmount)
  * [componentWillUpdate()](#componentwillupdate)
  * [constructor()](#constructor)
  * [defaultProps](#defaultprops)
  * [displayName](#displayname)
  * [forceUpdate()](#forceupdate)
  * [props](#props)
  * [render()](#render)
  * [setState()](#setstate)
  * [shouldComponentUpdate()](#shouldcomponentupdate)
  * [state](#state)
* [React.Fragment](#reactfragment)
* [React.PureComponent](#reactpurecomponent)

# Renderização de componentes react
Exemplo de renderização de componente react dentro do HTML:
```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

## cloneElement()
Clone e retorne o elemento React usando o elemento no ponto inicial. O elemento resultante terá os suportes originais. Novos filhos substituirão os filhos existentes. `key` e `ref` do elemento original serão preservados.
```js
React.cloneElement(
  element,
  [props],
  [...children]
)
```

## createElement()
Crie e retorne um novo elemento React do tipo especificado. O argumento `type` pode ser uma string de nome de tag (como `<div>` ou `<span>`), um tipo de componente React (uma classe ou uma função) ou um tipo de `React.Fragment`.

Você normalmente não chamará `React.createElement()` diretamente se estiver usando o JSX.
```js
React.createElement(
  type,
  [props],
  [...children]
)
//with JSX
class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.toWhat}</div>;
  }
}

ReactDOM.render(
  <Hello toWhat="World" />,
  document.getElementById('root')
);


//without JSX
class Hello extends React.Component {
  render() {
    return React.createElement('div', null, `Hello ${this.props.toWhat}`);
  }
}

ReactDOM.render(
  React.createElement(Hello, {toWhat: 'World'}, null),
  document.getElementById('root')
);
```

## isValidElement()
Verifica se o objeto é um elemento React. Retorna `true` ou `false`.
```js
React.isValidElement(object)
```

## React.Children
`React.Children` fornece utilitários para lidar com a estrutura de dados do `this.props.children`.
### React.Children.count
Retorna o número total de componentes filhos.
```js
React.Children.count(children)
```

### React.Children.forEach
Como `React.Children.map()`, mas não retorna um array.
```js
React.Children.count(children)
```

### React.Children.map
Traz uma função com todos os filhos imediatos contidos nos `children` com `this` conjunto para `thisArg`. Se `children` for um fragmento ou array, ele será atravessado: a função nunca será transmitida para os objetos contêineres. Se filhos for `null` ou `undefined`, retorna `null` ou `undefined` em vez de um array.
```js
React.Children.map(children, function[(thisArg)])
```

### React.Children.only
Verifica se `children` tem apenas um filho. Se não retornará `error`.
```js
React.Children.only(children)
```

### React.Children.toArray
Retorna `children` como um `array` com `keys` atribuídas a cada `child`. Serve para manipular coleções de `children` em seus métodos de `render`, especialmente se você quiser reordenar ou dividir `this.props.children`.
```js
React.Children.toArray(children)
```

## React.Component
É a classe base dos componentes do React quando eles são definidos usando classes ES6:

`React.Component` é uma classe base abstrata, por isso raramente faz sentido se referir diretamente ao React.Component. Em vez disso, você normalmente subclasse-o e define pelo menos um método `render()`.
```js
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Lifecycle do componente
São métodos prefixados com `will` são chamados logo antes de algo acontecer, e os métodos prefixados com `did` são chamados logo após algo acontecer.

#### Mounting
Esses métodos são chamados quando uma instância de um componente está sendo criada e inserida no DOM:
##### constructor()
O `constructor` no React é chamando antes do component ser montado. Quando geramos um constructor para um componente, você precisa chamar como `super(props)` antes de declarar alguma coisa. Diferente so `this.props`, que será `undefined` no `constructor` 
```js 
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
```

##### componentWillMount()
É chamando antes da montagem seja feita. É chamado antes do `render()`.
```js
componentWillMount()
```
##### render()
É chamado de forma obrigatória, ele examina `this.props` e `this.state` e retornar um dos seguintes tipos:

* Elementos: Um elemento pode ser uma representação de um componente DOM nativo (<div />) ou um componente composto definido pelo usuário (<MyComponent />);
* `String` and `Number`;
* Portais: Criado com ReactDOM.createPortal;
* `null`: Não faz nada;
* `Boolean`: Não faça nada. (Existe principalmente para suportar o teste de `return && <Child />` padrão, onde o teste é booleano)

Ao retornar `null` ou `false`, `ReactDOM.findDOMNode(this)` retornará `null`.

A função `render()` deve ser pura, o que significa que não modifica o estado do componente, retorna o mesmo resultado toda vez que é invocado e não interage diretamente com o navegador. Se você precisar interagir com o navegador, execute seu trabalho no `componentDidMount()` ou nos outros métodos do ciclo de vida. Manter o `render()` pure torna os componentes mais fáceis de pensar.

##### componentDidMount()
```js
componentDidMount()
```
Is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here. If you need to load data from a remote endpoint, this is a good place to instantiate the network request.

This method is a good place to set up any subscriptions. If you do that, don’t forget to unsubscribe in componentWillUnmount().

Calling setState() in this method will trigger an extra rendering, but it will happen before the browser updates the screen. This guarantees that even though the render() will be called twice in this case, the user won’t see the intermediate state. Use this pattern with caution because it often causes performance issues. It can, however, be necessary for cases like modals and tooltips when you need to measure a DOM node before rendering something that depends on its size or position.

#### Updating
Uma atualização pode ser causada por alterações em `props` ou `state`. Esses métodos são chamados quando um componente está sendo renderizado novamente:

##### componentWillReceiveProps()
```js
componentWillReceiveProps(nextProps)
```
`componentWillReceiveProps()` is invoked before a mounted component receives new props. If you need to update the state in response to prop changes (for example, to reset it), you may compare this.props and nextProps and perform state transitions using this.setState() in this method.

Note that React may call this method even if the props have not changed, so make sure to compare the current and next values if you only want to handle changes. This may occur when the parent component causes your component to re-render.

React doesn’t call componentWillReceiveProps() with initial props during mounting. It only calls this method if some of component’s props may update. Calling this.setState() generally doesn’t trigger componentWillReceiveProps().

##### shouldComponentUpdate()
```js
shouldComponentUpdate(nextProps, nextState)
```
Use shouldComponentUpdate() to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.

shouldComponentUpdate() is invoked before rendering when new props or state are being received. Defaults to true. This method is not called for the initial render or when forceUpdate() is used.

Returning false does not prevent child components from re-rendering when their state changes.

Currently, if shouldComponentUpdate() returns false, then componentWillUpdate(), render(), and componentDidUpdate() will not be invoked. Note that in the future React may treat shouldComponentUpdate() as a hint rather than a strict directive, and returning false may still result in a re-rendering of the component.

If you determine a specific component is slow after profiling, you may change it to inherit from React.PureComponent which implements shouldComponentUpdate() with a shallow prop and state comparison. If you are confident you want to write it by hand, you may compare this.props with nextProps and this.state with nextState and return false to tell React the update can be skipped.

We do not recommend doing deep equality checks or using JSON.stringify() in shouldComponentUpdate(). It is very inefficient and will harm performance.
##### componentWillUpdate()
```js
componentWillUpdate(nextProps, nextState)
```
componentWillUpdate() is invoked immediately before rendering when new props or state are being received. Use this as an opportunity to perform preparation before an update occurs. This method is not called for the initial render.

Note that you cannot call this.setState() here; nor should you do anything else (e.g. dispatch a Redux action) that would trigger an update to a React component before componentWillUpdate() returns.

If you need to update state in response to props changes, use componentWillReceiveProps() instead.

Note:
componentWillUpdate() will not be invoked if shouldComponentUpdate() returns false.

##### componentDidUpdate()
```js
componentDidUpdate(prevProps, prevState)
```
componentDidUpdate() is invoked immediately after updating occurs. This method is not called for the initial render.

Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to previous props (e.g. a network request may not be necessary if the props have not changed).

Note:
componentDidUpdate() will not be invoked if shouldComponentUpdate() returns false.

#### Unmounting
Esse método é chamado quando um componente é removido do DOM
##### componentWillUnmount()
```js
componentWillUnmount()
```
componentWillUnmount() is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any subscriptions that were created in componentDidMount().

#### Error Handling
Esse método é chamado e quando dá erro durante a renderização, no lifecycle do método, ou no `construtor` do filho do componente.

##### componentDidCatch()
```js
componentDidCatch(error, info)
```
Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

A class component becomes an error boundary if it defines this lifecycle method. Calling setState() in it lets you capture an unhandled JavaScript error in the below tree and display a fallback UI. Only use error boundaries for recovering from unexpected exceptions; don’t try to use them for control flow.

For more details, see Error Handling in React 16.

Note

Error boundaries only catch errors in the components below them in the tree. An error boundary can’t catch an error within itself.

### React.Fragment
The React.Fragment component lets you return multiple elements in a render() method without creating an additional DOM element:
```js
render() {
  return (
    <React.Fragment>
      Some text.
      <h2>A heading</h2>
    </React.Fragment>
  );
}
```
You can also use it with the shorthand <></> syntax. For more information, see React v16.2.0: Improved Support for Fragments.
### React.PureComponent
React.PureComponent is similar to React.Component. The difference between them is that React.Component doesn’t implement shouldComponentUpdate(), but React.PureComponent implements it with a shallow prop and state comparison.

If your React component’s render() function renders the same result given the same props and state, you can use React.PureComponent for a performance boost in some cases.

Note:
React.PureComponent’s shouldComponentUpdate() only shallowly compares the objects. If these contain complex data structures, it may produce false-negatives for deeper differences. Only extend PureComponent when you expect to have simple props and state, or use forceUpdate() when you know deep data structures have changed. Or, consider using immutable objects to facilitate fast comparisons of nested data.

Furthermore, React.PureComponent’s shouldComponentUpdate() skips prop updates for the whole component subtree. Make sure all the children components are also “pure”.

