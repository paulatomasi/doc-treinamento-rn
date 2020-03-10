# Componentes

Components e API - https://reactnative.dev/docs/components-and-apis.html

Componentes utilisados no treinamento:
  - Flatlist - https://reactnative.dev/docs/flatlist
  - View - https://reactnative.dev/docs/view
  - Button - https://reactnative.dev/docs/button
  - Stylesheet - https://reactnative.dev/docs/stylesheet
  - Image - https://reactnative.dev/docs/image
  - Text - https://reactnative.dev/docs/text

---

# Componentes customizados e plugins

## React Navigation

Ficou muito longo e ganhou uma session própria - [aqui](./react-navigation.md)

---

## IconSet / IcoMoon

https://icomoon.io/

No Icomoon é possivel importar vetores/icones e é gerado um pacote de fonts.

Nas configurações, associamos as fonts geradas com os gerenciamento dos icons feitos pelo plugin - https://github.com/oblador/react-native-vector-icons

Icon Sets disponiveis pelo plugin - https://github.com/oblador/react-native-vector-icons#bundled-icon-sets

Configurando IconSet do Icomoon - https://github.com/oblador/react-native-vector-icons#createiconsetfromicomoonconfig-fontfamily-fontfile

exemplo da doc. do plugin:
```js
  import { createIconSetFromIcoMoon } from 'react-native-vector-icons';
  import icoMoonConfig from './selection.json';
  const Icon = createIconSetFromIcoMoon(
    icoMoonConfig,
    'LineAwesome',
    'line-awesome.ttf'
  );
```

exemplo de instalação, customização do IconSet + Icomoon
https://www.reactnative.guide/12-svg-icons-using-react-native-vector-icons/12.1-creating-custom-iconset.html



### Configuração do Projeto
```js

import React from 'react'
import { createIconSetFromIcoMoon } from 'react-native-vector-icons'

import icoMoonConfig from 'assets/fonts/selection.json'

const Icon = createIconSetFromIcoMoon(icoMoonConfig, 'InstaIcons', 'InstaIcons.ttf')

export const IIcon = props => {
    return <Icon {...props} />
}

```

--- 

## FastImage

FastImage - https://github.com/DylanVann/react-native-fast-image

```js
import React, { PureComponent } from 'react'
import FastImage from 'react-native-fast-image'

export class IImage extends PureComponent {
    static RESIZE_MODE = {
        COVER: FastImage.resizeMode.cover,
        CONTAIN: FastImage.resizeMode.contain,
        STRETCH: FastImage.resizeMode.stretch,
        CENTER: FastImage.resizeMode.center,
    }

    render() {
        return (
            <FastImage
                style={this.props.style}
                source={this.props.source}
                resizeMode={this.props.resizeMode}
            />
        )
    }
}

```