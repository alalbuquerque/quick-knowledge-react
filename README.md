# Quick knowledge React
React é uma biblioteca pra construção de interfaces

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
* `Boolean`: Não faça nada. (Existe principalmente para suportar o teste de retorno && <Child /> padrão, onde o teste é booleano.)

Ao retornar null ou false, ReactDOM.findDOMNode (this) retornará null.

A função render () deve ser pura, o que significa que não modifica o estado do componente, retorna o mesmo resultado toda vez que é invocado e não interage diretamente com o navegador. Se você precisar interagir com o navegador, execute seu trabalho no componentDidMount () ou nos outros métodos do ciclo de vida. Manter o render () pure torna os componentes mais fáceis de pensar.

##### componentDidMount()
componentWillMount
#### Updating
Uma atualização pode ser causada por alterações em `props` ou `state`. Esses métodos são chamados quando um componente está sendo renderizado novamente:
##### componentWillReceiveProps()
##### shouldComponentUpdate()
##### componentWillUpdate()
##### render()
##### componentDidUpdate()

#### Unmounting
Esse método é chamado quando um componente é removido do DOM
##### componentWillUnmount()

#### Error Handling
Esse método é chamado e quando dá erro durante a renderização, no lifecycle do método, ou no `construtor` do filho do componente.
##### componentDidCatch()

### React.Fragment
### React.PureComponent


- React / Reference
- createPortal()
- DOM Elements
- findAllInRenderedTree()
- findDOMNode()
- findRenderedComponentWithType()
- findRenderedDOMComponentWithClass()
- findRenderedDOMComponentWithTag()
- Glossary of React Terms
- hydrate()
- isCompositeComponent()
- isCompositeComponentWithType()
- isDOMComponent()
- isElement()
- isElementOfType()
- JavaScript Environment Requirements
- mockComponent()
- props
- props.children
- React Top-Level API
- React.Component
- ReactDOM
- ReactDOMServer
- render()
- renderIntoDocument()
- renderToNodeStream()- 
- renderToStaticMarkup()
- renderToStaticNodeStream()
- renderToString()
- requestAnimationFrame
- scryRenderedComponentsWithType()
- scryRenderedDOMComponentsWithClass()
- scryRenderedDOMComponentsWithTag()
- Shallow Renderer
- shallowRenderer.getRenderOutput()
- shallowRenderer.render()
- Simulate
- state
- SyntheticEvent
- Test Renderer
- Test Utilities
- testInstance.children
- testInstance.find()
- testInstance.findAll()
- testInstance.findAllByProps()
- testInstance.findAllByType()
- testInstance.findByProps()
- testInstance.findByType()
- testInstance.instance
- testInstance.parent
- testInstance.props
- testInstance.type
- TestRenderer.create()
- testRenderer.getInstance()
- testRenderer.root
- testRenderer.toJSON()
- testRenderer.toTree()
- testRenderer.unmount()
- testRenderer.update()
- unmountComponentAtNode()
