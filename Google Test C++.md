# Google Test C++ for Linux(Ubuntu)

- [Google Test C++ for Linux(Ubuntu)](#google-test-c-for-linuxubuntu)
  - [Google Test C++のインストール](#google-test-cのインストール)
  - [単体テスト用プログラム作成](#単体テスト用プログラム作成)
  - [初歩的なテストコードの書き方](#初歩的なテストコードの書き方)
    - [テスト関数](#テスト関数)
    - [アサーション](#アサーション)
  - [実際のソフトでの単体テストの作成方法](#実際のソフトでの単体テストの作成方法)
    - [依存する関数のMock化](#依存する関数のmock化)
  - [テストでモックを使う](#テストでモックを使う)

## Google Test C++のインストール

MakeとCMakeが必要なので、インストールしていない場合はインストールすること

```bash
cd /tmp
$ wget 'https://github.com/google/googletest/archive/release-1.8.1.tar.gz'
$ tar zxvf release-1.8.1.tar.gz
$ mkdir -p /usr/local/src
$ mv googletest-release-1.8.1 /usr/local/src
$ cd /usr/local/src/googletest-release-1.8.1
$ mkdir build
$ cd build
$ cmake ..
$ make
$ make install
```

## 単体テスト用プログラム作成

```h
#ifndef SAMPLE_H_
#define SAMPLE_H_

/**
* 入力値が偶数か判定する関数
*/
bool IsEven(int x);

#endif  // SAMPLE_H_
```

```cpp
#include "sample.h"

bool IsEven(int x)
{
    return x % 2 == 0;
}
```

```cpp
#include <gtest/gtest.h>

#include "sample.h"

TEST(IsEvenTest, Negative)
{
    EXPECT_FALSE(IsEven(-1));
    EXPECT_TRUE(IsEven(-2));
}

TEST(IsEvenTest, Zero)
{
    EXPECT_TRUE(IsEven(0));
}

TEST(IsEvenTest, Positive)
{
    EXPECT_FALSE(IsEven(1));
    EXPECT_TRUE(IsEven(2));
}
```

次のコマンドでコンパイルを行う

```bash
g++ -std=c++11 sample.cc sample_test.cc -o test -L/usr/local/lib -lgtest -lgtest_main
```

## 初歩的なテストコードの書き方

### テスト関数

Google Testに用意されているTEST()マクロを使用する

```cpp
TEST(/* テストケース名(大項目)*/, /* テスト名(小項目) */)
{
  // テスト関数内は、通常通り C++ のコードを記述可能
}
```

テストケース名とテスト名に_を含んではいけない

### アサーション

Google Testに用意されているアサーションを利用することで、テスト対象コードの動作を検証することができる

```cpp
// true/falseのアサーション
EXPECT_TRUE(condition);  // condition が true か
EXPECT_FALSE(condition);  // condition が false か

// 2つの値を比較するアサーション
EXPECT_EQ(expected, actual);  // expected == actual か
EXPECT_NE(expected, actual);  // expected != actual か
EXPECT_LT(expected, actual);  // expected < actual か
EXPECT_LE(expected, actual);  // expected <= actual か
EXPECT_GT(expected, actual);  // expected > actual か
EXPECT_GE(expected, actual);  // expected >= actual か
```

## 実際のソフトでの単体テストの作成方法

実際のソフトでは、各クラス同士が依存し、1つの関数の単体テストを作成しようにも容易にできない。
そこでGoogle TestではGoogle Mockを使用する。
Google Mockは依存するクラスをMock化してくれる機能を提供している。

### 依存する関数のMock化

例えばDBアクセス用クラスの場合、以下のように定義する

```cpp
// 元のコード
bool GetInformation(int id, Information& information);
// Mock化後のコード
// GetInoformation_mock.h
#include "gmock/gmock.h"
MOCK_METHOD2(GetInformation, bool(int id, Information& information));
```

MOCK_METHODn nは引数の数を指す
Mockクラスの定義をどうするかは、チーム内で決める必要があるが、推奨する方法は以下

* Mock化するクラスのパッケージ内でMockクラスを定義する。
   Fooインターフェースとした場合、mock_foo.hを作成する

* 製品版コードとテストユーティリティ別プロジェクトで管理する。

## テストでモックを使う

1. 名前空間の定義
2. モックオブジェクトの作成
3. Expectationを定義する
4. モックを利用したコードを実行する
5. モックがデストラクタされると、その全てのExpectationが成功したかどうかをGoogle Mockが自動的に検証する

```cpp
#include "path/to/mock-turtle.h"
#include "gmock/gmock.h"
#include "gtest/gtest.h"
using ::testing::AtLeast;                     // #1
   
TEST(PainterTest, CanDrawSomething) {
  MockTurtle turtle;                          // #2
  EXPECT_CALL(turtle, PenDown())              // #3
      .Times(AtLeast(1));
   
  Painter painter(&turtle);                   // #4
   
  EXPECT_TRUE(painter.DrawCircle(0, 0, 10));
}                                             // #5
   
int main(int argc, char** argv) {
  // 以下の行は，テスト開始前に Google Mock （と Google Test）
  // を初期化するために必ず実行する必要があります．
  ::testing::InitGoogleMock(&argc, argv);
  return RUN_ALL_TESTS();
}
```

* 重要な注意事項

   <span style="color: red;">Google Mockでは、モック関数が呼び出される前にExpectationが設定される必要がある。
   そうでない場合の挙動は未定となっている。
   特にEXPECT_CALL()とモック関数の呼び出しをごちゃまぜにするのは、決してやってはいけない。</span>
