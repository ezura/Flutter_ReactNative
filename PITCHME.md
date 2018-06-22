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
* Native code 呼び出さない前提なら [Expo](https://expo.io/) を使うと便利
  
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

---

##  Native の View の中に埋め込めるのか

### flutter
* 可能 (文書化はされてない)

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

## Design

### flutter
* 基本的に Material design
  - iOS に合わせた Widget 群もある
    - [Cupertino (iOS-style) Widgets  - Flutter](https://flutter.io/widgets/cupertino/)
    - [Building Beautiful UIs with Flutter](https://codelabs.developers.google.com/codelabs/flutter/#3)
* 用意されている独自の UI 群 (Widget) を使うので、カスタマイズ可能な部分は Native と勝手が異なる
  - 用意されている Widget でそのデザインは実現できるかの確認が必要
  - デザイナーさんとの合意が必要

### React Native
* Native の UI を使うので違和感は少ない
  - Native code で作った View を埋め込むこともできる

---

## React Native アプリを Native アプリに戻すことができるか（容易か）

### flutter
* けっこうコストかかりそう
  - 書き直し
  - 独自の UI component を再現できるか (特に iOS)
  - native の SDK と思想が異なる
    - React っぽい

### React Native
* ややコストかかりそう
  - 書き直し
  - native の SDK と思想が異なる
    - React

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

## 環境毎に設定切り替えられるか

### flutter
*  [環境ごとの設定](https://qiita.com/gki/items/db9bac1e6eece826f564#%E7%92%B0%E5%A2%83%E3%81%94%E3%81%A8%E3%81%AE%E8%A8%AD%E5%AE%9A)

### React Native
*  [react-native-dotenv で定数を環境ごとに切り替える](https://qiita.com/hotchpotch/items/f5661def49e0f54ec276)

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
* [continuous integration - CI tool for React Native - Stack Overflow](https://stackoverflow.com/questions/47169137/ci-tool-for-react-native)
* [Bitrise 使う方法](https://qiita.com/jtakahashi64/items/5133358aa55a03137fbc)

---

# 評判
* [Android / iOS アプリの開発にクロスプラットフォームの Flutter を実戦投入した｜najeira｜note](https://note.mu/najeira/n/n8924408dd07b)
* [Flutter vs React Native- What You Need To Know. – Dmytro Dvurechenskyi from openGeeksLab – Medium](https://medium.com/@openGeeksLab/flutter-vs-react-native-what-you-need-to-know-89451da3c90b)
* [React Native, Flutter, Xamarin: a comparison](https://blog.novoda.com/react-native-flutter-xamarin-a-comparison/)
* [React NativeでiOSアプリを作ってストアでリリースしてみた](https://qiita.com/ariiyu/items/81154c45b8b37feb8009)
* [Sunsetting React Native – Airbnb Engineering & Data Science – Medium](https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a)
* [いつ ReactNative を使っても大丈夫か - mizchi’s blog](http://mizchi.hatenablog.com/entry/2018/06/20/115539)

+++

## flutter

### 良い点
* 静的な型付け
* async / await 
* OS 間での UI の差がない
* Widget は充実している
* 70% くらい flutter で書けている
  - UI 周りの実装が中心のアプリ
  - デバイス周り(カメラとか　bluetooth とか)での実装が必要だと割合下がる
* Google の出しているアプリの一部は flutter で実装されている

### 欠点
* まだバグが多い
  - issue は 2000 以上
* 情報が少ない
  - flutter の中身を読んで理解
* ライブラリがまだ充実してない
* Native の View とは一つの画面で混在するのはできるけど難しい
* デザイナーさんに flutter が得意なデザインを知ってもらう必要がある

---

## React Native

### 良い点
* ライブラリが充実している
* Web のエンジニアにとって導入コストが低かった
* Instagram や facebook でも使われている

### 欠点
* Android と iOS のどちらかでバグっているライブラリも多い
* 速く開発できたが、技術的な負債をかかえていった
  - React Native で実現しにくいから止めた仕様
  - Native code との bridge を書くことになる
* 初期のレンダリングが遅い
* Airbnb は React Native のコードを Native に書き換える作業をしている
  - 「最初は Native のエンジニアが足りなかったから React Native を使っていたけれど、今は Native のエンジニアいるので無理して React Native 使わなくても Native で作れるし…」

---

# まとめ

## flutter
* Dart
  - 静的型付け (動的っぽいとは思うけど…)
* まだまだバグ多い
* iOS 向けのデザインもある
* デザインに制約がかかる
  
## React Native
* Native の UI を使う
* JavaScript
* 安定してきてる

## 共通
* bridge 書くのと保守するの大変
* デバイス周りの処理が多い場合は共通化するのが難しくなる
  - UI なら共通化できるし活躍できる

