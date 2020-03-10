
# Class Component

```javascript
import React, { Component } from 'react'
import {Text, View} from 'react-native'

class basic extends Component {
    // Método obrigatório para class components
    render() {
        return <View><Text>Estrutura Básica de um component</Text></View>
    }
}

export default basic

```

Ao herdar de `Component` passamos também a ter acesso aos métodos do ciclo de vida do componente.

```javascript
class basic extends Component {
    // Não está atrelado ao Ciclo de Vida do component
    constructor(props) {
        super(props)
    }

    // Executado após o constructor. ANTES do Render
    componentWillMount() {}

    // Método chamado para construir a View do component.
    // Sempre é invocado novamente a cada alteração nas Props ou State do component
    // Retorna um JSX
    render() {
        return (<jsx>View do Component</jsx>)
    }

    // Executado após o Render da View do component
    componentDidMount() {}

    // Chamado quando alterado Props ou State. Executado ANTES do Render
    componentWillUpdate(newProps, newState) {}

    // Chamado quando alterado Props ou State. Executado APÓS do Render
    componentDidUpdate() {}

    // Executado antes de desmontar um component
    componentWillUnmount() {}

}
```

Com Class component temos acesso a `Props` e `State` como propriedades da Classe.

```javascript
class basic extends Component {
    render() {
        return (<View><Text>{this.props.title - this.state.nameUser}</Text></View>)
    }

    updateNameUser(name) {
        this.setState({ nameUser: name })
    }
}

```

Passando `Props` e `State`

```javascript
export class MovieCard extends Component {
    render() {
        return (
                <View>
                    <Text>Titulo Original:{this.props.title}<Text>
                    <Text>Ano de Lançamento:{this.props.year}<Text>
                    <Text>Diretor:{this.props.director}<Text>
                <View>
    }
}
```

```javascript
export class MovieList extends Component {
    constructor() {
        this.state = [
            movies: ...this.service.getMovies()
        ]
    }
    render() {
        return {this.state.movies.map((movie) =>
            <MovieCard
                title={movie.title}
                year={movie.year}
                director={movie.director}>
            </MovieCard>)}
    }
}
```
