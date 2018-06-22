# Flutter / React Native

---

* それぞれの特徴
* 疑問点の調査結果
* 評判
* まとめ

---

## [flutter](https://flutter.io/)

* Google 製
* 使用言語: [Dart](https://www.dartlang.org/)
* hot reload
* 開発環境: AndroidStudio or VS Code
* View に flutter が自前で UI をレンダリングする
  - 基本的に material design

+++

### [Dart](https://www.dartlang.org/)

* Java に似てる
* type safe
  - とはいえ至る所に `dynamic` (型情報のない型) が出てくる
* null safety: 実装中ってずっと言ってる
  - `.?` とか `??` はある
* 型推論あり
* 突飛な言語仕様ではないので言語の習得コストは少ない
  - 一瞬戸惑うとしたら async/await くらい？

+++

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Welcome to Flutter',
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Welcome to Flutter'),
        ),
        body: new Center(
          child: new Text('Hello World'),
        ),
      ),
    );
  }
}
```

---

## [React Native](https://facebook.github.io/react-native/)

* facebook 製
* 使用言語: JavaScript, (JSX)
* hot reload
* 開発環境: 何でも良い
  - Node 使う
* UI は Native の UI を使う
  - native の View を wrap　している
* iOS: やり方によっては審査通さずにアップデート可能
  
+++

```javascript
import React, { Component } from 'react';
import { Image, ScrollView, Text } from 'react-native';

class AwkwardScrollingImageWithText extends Component {
  render() {
    return ( // ここから JSX
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

---

# 疑問点の調査結果

---

## Native code を呼び出せるか

### flutter
* 可能
* https://flutter.io/platform-channels/

### React Native
* 可能
* https://facebook.github.io/react-native/docs/communication-ios.html
* https://facebook.github.io/react-native/docs/native-modules-android.html

---

## 一部の画面でだけ使うことはできるか

### flutter
* 可能
* https://flutter.io/faq/#can-i-use-flutter-inside-of-my-existing-native-app
* https://github.com/flutter/flutter/issues/13828

### React Native
* 可能
* https://github.com/facebook/react-native/issues/1148

---

## Native の View を中で使えるのか

### flutter
* 不可能
* (flutter が描画している View の上に乗せることは可能)

### React Native
* 可能

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

---

## ローカライズ

### flutter
* plugin 使えば少しは楽
  - https://qiita.com/jiro-aqua/items/fd9896682ca09018fdd3
  - 一度コマンド叩いてローカライズ対象毎に必要なコードを生成する

### React Native
* ライブラリ使う
  - https://medium.com/@danielsternlicht/adding-localization-i18n-g11n-to-a-react-native-project-with-rtl-support-223f39a8e6f2
  - json に書く

---

## 難読化

### flutter
* Release build では標準で難読化される
* Native code の難読化時は flutter が影響を受けないように設定する必要がある

### React Native
* Release build では標準で minify される

---

## CI

### flutter
* [Continuous Delivery using Fastlane with Flutter](https://flutter.io/fastlane-cd/)

### React Native
* [Bitrise 使う方法](https://qiita.com/jtakahashi64/items/5133358aa55a03137fbc)
* ノウハウは溜まっている模様

---

## Design

### flutter
* Material design に引っ張られる
  - Material 統一
### React Native
