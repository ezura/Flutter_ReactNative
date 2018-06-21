# Flutter / React Native

---

* それぞれの特徴
* 比較
* 疑問点の調査結果
* 評判

---

## [flutter](https://flutter.io/)

* Google 製
* 使用言語: Dart
* hot reload
* 開発環境: AndroidStudio or VS Code
* View に flutter が自前で UI をレンダリングする

+++

### [Dart](https://www.dartlang.org/)

* Java に似てる
* type safe
* null safety ではない
* 型推論あり
* 突飛な言語仕様ではないので言語の習得コストは少ない
  - async/await くらい？

---

## [React Native](https://facebook.github.io/react-native/)

* facebook 製
* 使用言語: JavaScript, (JSX)
* hot reload
* 開発環境: 何でも良い
  - Node 使う
  
+++

```javascript
import React, { Component } from 'react';
import { Image, ScrollView, Text } from 'react-native';

class AwkwardScrollingImageWithText extends Component {
  render() {
    return (
      <ScrollView>
        <Image
          source={{uri: 'https://i.chzbgr.com/full/7345954048/h7E2C65F9/'}}
          style={{width: 320, height:180}}
        />
        <Text>
          On iOS, a React Native ScrollView uses a native UIScrollView.
          On Android, it uses a native ScrollView.

          On iOS, a React Native Image uses a native UIImageView.
          On Android, it uses a native ImageView.

          React Native wraps the fundamental native components, giving you
          the performance of a native app, plus the clean design of React.
        </Text>
      </ScrollView>
    );
  }
}
```


+++

自作の native の View `TheGreatestComponentInTheWorld` 使いたい場合

```javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';
import { TheGreatestComponentInTheWorld } from './your-native-code';

class SomethingFast extends Component {
  render() {
    return (
      <View>
        <TheGreatestComponentInTheWorld />
        <Text>
          TheGreatestComponentInTheWorld could use native Objective-C,
          Java, or Swift - the product development process is the same.
        </Text>
      </View>
    );
  }
}
```

| |flutter |React Native |
|---|---|---|
|language |Dart |JavaScript  |
|4  |5  |
