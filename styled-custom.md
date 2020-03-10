# Estilização

Cheatsheet - https://github.com/vhpoet/react-native-styling-cheat-sheet

Extended StyleSheet - https://github.com/vitalets/react-native-extended-stylesheet

Configuração do Theme

```js
import EStyleSheet from 'react-native-extended-stylesheet'

const colors = {
    $white: '#FFFFFF',
    $background: '#FAFAFA',
    $lightGrey: '#D9D9D9',
    $mediumGrey: '#999999',
    $darkGrey: '#262626',
    $blue: '#3797EF',
}

const statusBar = {
    light: 'light-content',
    dark: 'dark-content',
}

export class Theme {
    static COLORS = colors
    static STATUS_BAR = statusBar

    static build() {
        EStyleSheet.build({ $colors: colors }) // Adiciona as variáveis de cores ao escopo
    }
}


// App.js
import { AppRegistry } from 'react-native'
import App from './App'
import { Theme } from 'theme'

Theme.build()
AppRegistry.registerComponent('insta', () => App)


```

exemplo de customização:

```js
import React, { Component } from 'react'
import EStyleSheet from 'react-native-extended-stylesheet'
import { View, Text } from 'react-native'

import { INavBar, IImage } from 'components'

export class HomeScreen extends Component {
    render() {
        return (
            <View style={{ flex: 1 }}>
                <INavBar />
                {/* HEADER */}
                <View style={Styles.headerContainer}>
                    <IImage
                        style={Styles.userImage}
                        source={{ uri: 'https://observatoriog.bol.uol.com.br/wordpress/wp-content/uploads/2019/11/rihanna.jpg' }}
                        resizeMode={IImage.RESIZE_MODE.COVER}
                    />
                    <View>
                        <Text style={Styles.username}>badgalriri</Text>
                        <Text style={Styles.local}>New York</Text>
                    </View>
                </View>
                {/* LIKES */}
                <View style={Styles.likesContainer}>
                    <Text style={Styles.likesText}>
                        Liked by
                        <Text style={Styles.boldText}> craig_love </Text>
                        and
                        <Text style={Styles.boldText}> others</Text>
                    </Text>
                </View>
            </View>
        )
    }
}



const Styles = EStyleSheet.create({
    headerContainer: {
        height: 54,
        backgroundColor: '$colors.$white',
        flexDirection: 'row',
        alignItems: 'center',
    },
    userImage: {
        height: 32,
        width: 32,
        borderRadius: 16,
        marginHorizontal: 10,
    },
    username: {
        fontSize: 13,
        fontWeight: 'bold',
        color: '$colors.$darkGrey',
    },
    likesContainer: {
        marginLeft: 15,
    },
    local: {
        fontSize: 11,
        color: '$colors.$darkGrey',
    },
    likesText: {
        fontSize: 13,
        color: '$colors.$darkGrey',
    },
    boldText: {
        fontWeight: 'bold',
    },
})

export default Styles
```