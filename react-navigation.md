# React Navigation
utilizamos a versão 4.x. Instalação completa - https://reactnavigation.org/docs/4.x/getting-started

Navigation Events - https://reactnavigation.org/docs/4.x/navigation-events

Stack Actions - https://reactnavigation.org/docs/4.x/stack-actions

Dependencias:

yarn add react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view react-navigation-stack

```js
import React from 'react'
import { createAppContainer } from 'react-navigation'
import { createStackNavigator } from 'react-navigation-stack'
import { createBottomTabNavigator } from 'react-navigation-tabs'

import {
  HomeScreen,
  // ...
} from 'screens'
import { ITabBar, IIcon } from 'components'
import { Theme } from 'theme-consts'

const TabsHome = createBottomTabNavigator(
  {
    HomeScreen: {
      screen: HomeScreen, // Component da tela
      navigationOptions: {
        tabBarIcon: ({ tintColor }) => ( // Icone adicionado a tabBar (inferior)
          <IIcon name="home_icon" style={{ color: tintColor, fontSize: 32 }} />
        ),
      },
    },
    // ...
  },
  {
    tabBarComponent: ITabBar, // Component de tabBar (ex. abaixo)
    tabBarOptions: {
      activeTintColor: Theme.COLORS.$darkGrey,
      inactiveTintColor: Theme.COLORS.$mediumGrey,
    },
    lazy: true,
    swipeEnabled: false,
    animationEnabled: false,
  },
)

const AppNavigator = createStackNavigator(
  {
    Tabs: {
      screen: TabsHome,
    },
    // ...
  },
  { headerMode: 'none' },
)

export default createAppContainer(AppNavigator)

```

### NavBar
NavBar padrão foi ocultada e sobreescrita pelo componente abaixo.

Neste componente é possível ver a customização da StatusBar


```js
import React, { Component } from 'react'
import { StatusBar, View, Text } from 'react-native'

import { IImage, ITouchable, IIcon } from 'components'
import logoImage from 'assets/images/logo.png'

import Styles from './i-nav-bar.style'

export class INavBar extends Component {
    // StatusBar Customizada
    constructor(props) {
        super(props)

        StatusBar.setTranslucent(true)
        StatusBar.setBackgroundColor('transparent')
    }

    static setLightContent() {
        StatusBar.setBarStyle('light-content')
    }

    static setDarkContent() {
        StatusBar.setBarStyle('dark-content')
    }
    //

    render() {
        return (
            <View style={Styles.container}>
                <ITouchable style={Styles.buttonContainer}>
                    <IIcon name={'camera_icon'} style={Styles.icon} />
                </ITouchable>
                <IImage
                    source={logoImage}
                    resizeMode={IImage.RESIZE_MODE.CONTAIN}
                    style={Styles.logo}
                />
                <ITouchable style={Styles.buttonContainer}>
                    <IIcon name={'direct_icon'} style={Styles.icon} />
                </ITouchable>
            </View>
        )
    }
}

```

### TabBar
https://reactnavigation.org/docs/4.x/tab-based-navigation

```js
import React, { PureComponent } from 'react'
import { TouchableWithoutFeedback, View, Text } from 'react-native'

import { NavigationService } from 'services'

import Styles from './i-tab-bar.style'

export class ITabBar extends PureComponent {
    constructor(props) {
        super(props)

        this.state = {
            OnStatusBarFrameChange: false,
        }

        this.changeTab = this.changeTab.bind(this)
        this.renderTabBarButton = this.renderTabBarButton.bind(this)
    }

    changeTab(idx, route, instance) {
        const { navigation } = this.props

        if (navigation.state.index !== idx) {
            NavigationService.goTo(instance, route.routeName)
        } else {
            NavigationService.popToTop(instance)
        }
    }

    renderTabBarButton(route, idx) {
        const {
            activeTintColor,
            inactiveTintColor,
            navigation,
            renderIcon,
        } = this.props

        const color =
            navigation.state.index === idx ? activeTintColor : inactiveTintColor

        return (
            <TouchableWithoutFeedback
                onPress={() => {
                    this.changeTab(idx, route, this)
                }}
                key={route.routeName}
            >
                <View style={Styles.tabButtonContainer}>
                    {renderIcon({ route, tintColor: color })}
                </View>
            </TouchableWithoutFeedback>
        )
    }

    render() {
        const { navigation } = this.props
        const tabBarButtons = navigation.state.routes.map(this.renderTabBarButton)

        // Sorry About That, but i can`t find other solution for that
        // In-Call Status Bar, is don`t recognized from react-native
        // Native Bridge send event from React-Native listened on TabBar to adjust bottom bar.
        let OnStatusBarFrameChangeStyle = this.state.OnStatusBarFrameChange
            ? { bottom: 20 }
            : { bottom: 0 }

        return (
            <View style={[Styles.tabBarContainer, OnStatusBarFrameChangeStyle]}>
                {tabBarButtons}
            </View>
        )
    }
}


```

### Service
Classe que encapsula as StackActions - https://reactnavigation.org/docs/4.x/stack-actions

```js

import { StackActions, NavigationActions } from 'react-navigation'

export class NavigationService {
    static goBack(instance, screensToGoBack) {
        instance.props.navigation.pop(screensToGoBack)
    }

    static goTo(instance, screen, params = []) {
        instance.props.navigation.navigate(screen, params)
    }

    static push(instance, screen, params = []) {
        instance.props.navigation.push(screen, params)
    }

    static resetStack(instance, routeName, params = []) {
        const navigateAction = StackActions.reset({
            index: 0,
            actions: [NavigationActions.navigate({ routeName, params })],
        })

        instance.props.navigation.dispatch(navigateAction)
    }

    static popToTop(instance) {
        instance.props.navigation.dispatch(StackActions.popToTop())
    }
}


```

