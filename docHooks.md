# Utilizando Hooks

Primeiramente, o que são os Hooks? Hooks são uma nova adição ao React 16.8. Eles permitem que você use o state e outros recursos do React sem escrever uma classe.
Hooks resolvem uma variedade de problemas aparentemente separados em React que encontramos ao longo de cinco anos escrevendo e mantendo milhares de componentes. Esteja você aprendendo React, usando diariamente, ou até mesmo se prefere outra biblioteca com um modelo de componente parecido, você reconhecerá alguns destes problemas.


# Use State
Para começar, vamos ver o funcionamento básico do useState!
Este exemplo renderiza um contador. Quando você clica no botão, ele incrementa o valor:

```javascript
function Example() {
  const [count, setCount] = useState(0);
return (
<View>
<Text>Você clicou {count} vezes </Text>
<Button onPress={()=> setCount(count + 1)}>Aperte</Button>
</View>
);
}
```

Aqui, useState é um Hook. Nós o chamamos dentro de um componente funcional para adicionar alguns states locais a ele. O React irá preservar este state entre re-renderizações. useState retorna um par: o valor do state atual e uma função que permite atualizá-lo. Você pode chamar essa função a partir de um manipulador de evento ou de qualquer outro lugar. É parecido com this.setState em uma classe, exceto que não mescla o antigo state com o novo.
O único argumento para useState é o state inicial. No exemplo acima, é 0 porque nosso contador começa do zero. Perceba que diferente de this.state, o state não precisa ser um objeto — apesar de que possa ser se você quiser. O argumento de state inicial é utilizado apenas durante a primeira renderização.

Você pode utilizar o State Hook mais de uma vez em um único componente, sem problemas!

```javascript
import React, {useState} from 'react';
function ExampleWithManyStates() {
  // Declara várias variáveis de state!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```
Vamos ver um exemplo equivale utilizando classes:

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

# UseEffect
Você provavelmente já realizou obtenção de dados (data fetching), subscrições (subscriptions) ou mudanças manuais no DOM através de componentes React antes. Nós chamamos essas operações de “efeitos colaterais” (side effects ou apenas effects) porque eles podem afetar outros componentes e não podem ser feitos durante a renderização.

O Hook de Efeito, useEffect, adiciona a funcionalidade de executar efeitos colaterais através de um componente funcional. Segue a mesma finalidade do componentDidMount, componentDidUpdate, e componentWillUnmount em classes React, mas unificado em uma mesma API
Quando você chama useEffect, você está dizendo ao React para executar a sua função de “efeito” após liberar as mudanças para o DOM. Efeitos são declarados dentro do componente, para que eles tenham acesso as suas props e state. Por padrão, React executa os efeitos após cada renderização — incluindo a primeira renderização.

Por exemplo, este componente define o título da página após o React atualizar o DOM:

```javascript
import React, { useState, useEffect } from 'react';
function Example() {
  const [count, setCount] = useState(0);

  // Similar a componentDidMount e componentDidUpdate:
  useEffect(() => {
    // Atualiza o título do documento utilizando a API do navegador
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
Vamos ver um exemplo equivalente utilizando classes:

```javascript
class Exemplo extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `Você clicou ${this.state.count} vezes`;
  }

  componentDidUpdate() {
    document.title = `Você clicou ${this.state.count} vezes`;
  }

  render() {
    return (
      <div>
        <p>Você clicou {this.state.count} vezes</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

Efeitos também podem opcionalmente especificar como “limpar” (clean up) retornando uma função após a execução deles. Por exemplo, este componente utiliza um efeito para se subscrever ao status online de um amigo e limpa-se (clean up) cancelando a sua subscrição:

```javascript
import React, { useState, useEffect } from 'react';
function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

Assim como useState, você pode utilizar mais de um efeito em um componente:

```javascript
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // ...
  ```

 # ✌️  Regras dos Hooks
 Apenas chame Hooks no nível mais alto. Não chame Hooks dentro de loops, condições ou funções aninhadas.
Apenas chame Hooks de componentes funcionais. Não chame Hooks de funções JavaScript comuns. 


links úteis: https://pt-br.reactjs.org/ , https://pt-br.reactjs.org/docs/hooks-state.html , https://pt-br.reactjs.org/docs/hooks-effect.html
Bibliografia: https://pt-br.reactjs.org/
