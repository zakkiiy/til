# データとデータ型
## プリミティブ型で、不変（immutable）
- number, string, boolean, undefined, null, symbol, bigint
- 値そのものを保存
- 値がコピーされる

```typescript
let str = 10;
let str2 = str;
str2 = 20;
console.log(str); // 10
console.log(str2); // 20
```
- let str2 = str; の時点で、str2 に str の「値」だけをコピーしているので、その後は str とは無関係。
- つまり、str2 は str を見ているのではなく、「値」だけを持っている 状態。

## リファレンス型で、可変（mutable）
-  object, array, function, map, set
-  参照（メモリのアドレス）を保存
-  参照がコピーされる（同じオブジェクトを指す）
```typescript
let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2.push(4);
console.log(arr1); // [ 1, 2, 3, 4 ]
console.log(arr2); // [ 1, 2, 3, 4 ]

let obj1 = { name: "Alice" };
let obj2 = obj1;
obj2.name = "Bob";
console.log(obj1.name); // "Bob"
```
- arr2 = arr1; は、「arr1 が指しているメモリ上のオブジェクトの参照」をコピーしている だけです。
- arr1 そのもの（オブジェクトの参照）をコピーしたので、変更すると 元のデータ（メモリ上の配列）が変わり、arr1 も影響を受ける。

# 宣言と型アノテーション
- JSにおいて、変数そのものは型を持たず、どのような値でも代入可能
- TSでは、変数が型を持ち、初期化に初期化に使った値を元に文脈的型付けを行う。
  - '文字列'を代入すると文字列型など
  - 型推論による型付けとなる。
- 明示的な型指定もできる。（型アノテーション）
  - `let count: number = 10;`
```ts
const wordList1 = ["apple", "banana", 7];
// (string | number)[] 型として、型推論する

const wordList2: string[] = ["apple", "banana", 7];
// 型 'number' を型 'string' に割り当てることはできません。
// 以下のように型をつけておくとOK
const wordList3: (string | number)[] = ["apple", "banana", 7];
```

## 関数の仮引数
- 関数の仮引数には型アノテーションで型を明示する
- 返り値には、関数本体での処理を元に型推論するため、変数と同じく必須ではない。
```js
// 返り値はnumber型と推論される
function addOne(n: number) {
    return n + 1;
}

// 返り値はあえて、string型を明示
function addTwo(n: string): string {
  return n + "Two";
}

// 関数は変数に代入することも可能　無名関数
const addThree = function (n: number) {
  return n + 1;
}

// アロー関数　短く書ける　無名関数
const addFor = (n: number) => {
  return n + 1;
}

// 関数本体が1行の場合、{}とreturnを省略できる
const addFive = (n: number) => n + 1;

// 返り値がない場合は戻り値をvoidと指定できる
// undefined を返す場合も
function logMessage(message: string): void {
  console.log(message);
}
```
## オブジェクトとオブジェクトの型
```js
// 値としてのオブジェクト
const mail1 = {
  name: "通常はがき",
  price: 85,
}

// 型エイリアスを使ったオブジェクトの型定義
type MailType = {
  name: string;
  price: number;
}

// 型アノテーションされた変数を宣言してオブジェクトを代入
const mail2: MailType = {
  name: "メール便",
  price: 120,
}
```

# 制御フローと反復処理
## if文
```js
// if
if (0.3 === 2/7) {
  console.log("真");
} else if (0.3 > 2/7) {
  console.log("0.3の方が大きい");
} else {
  console.log("どちらでもない");
}
// 0.3の方が大きい
```

## try...catchを使用したエラー処理（エラーハンドリング）
- WebAPIからエラーレスポンスが返却されたりするとプログラム実行中に例外と言われるエラーが発生する。tryブロックの中で、エラーが発生し、そして補足（catch）されると処理は中断されてcatchブロックに移行する。丸括弧（）で指定した変数で参照可能
```js
try {
  const readUndefined = undefined;
  console.log("結果", readUndefined);
} catch (error) {
  console.error("エラー", error);
}
```
## コールバック関数
- コールバック関数とは、「引数に関数を渡すこと」
- つまり、「関数を引数として渡し、その関数があとで実行される」 というのが コールバック関数の基本的な動作

```js
function sayHello() {
  console.log("Hello");
}

function greet(n) {
  n();
}

greet(sayHello);
```

```js
const numbers = [2,3,5];
const numbersPlusTwo = numbers.map((n) => {
  return n + 2;
})

console.log(numbersPlusTwo); // [ 4, 5, 7 ]
```
- map に渡している (n) => n + 2 は「無名関数」
- function キーワードを使わずに定義している
- 関数名がない
- map の引数として直接渡されている

## map に渡す関数を「名前付き関数」にすると？
```js
const numbers = [2, 3, 5];

function addTwo(n) {
  return n + 2;
}

const numbersPlusTwo = numbers.map(addTwo);
console.log(numbersPlusTwo); // [4, 5, 7]
```

# undefinedとオプショナル
- JSで存在しないプロパティにアクセスするとundefinedという値になるが、オプショナルという概念で扱いやすくなった。
1. オブジェクトのプロパティには.で次々とアクセス可能
2. .?でチェーンするとundefinedやnullといった値にアクセスしても参照エラーとならない
3. TSではオプショナルプロパティやオプショナル引数が利用可能

チェーン演算子（.）でオブジェクトのプロパティにアクセスして値を取得したり、関数であれば呼び出し可能
```ts
const greeting = 'Hello World'
const charLength = greeting.length
console.log(charLength) // 11
// メソッドチェーン
const greetingString = greeting.toUpperCase().replace(" ", "!")
console.log(greetingString) // HELLO!WORLD
```

```ts
// gender の型は "female" | "male"（リテラル型のユニオン）
// gender には "female" または "male" のどちらかしか入れられない
// それ以外の文字列や number などはエラーになります。
// gender? の ? は「オプショナルプロパティ」と呼ばれ、そのプロパティが「存在しなくてもOK」になる ことを意味します。
type Citizen = {
  name: string;
  age: number;
  gender?: "female" | "male";
}
```
## リテラル型と型の絞り込み
- constアサーションを使用するとリテラル型となる
```ts
const greeting = "hello" as const;
```

## Promiseとジェネリクス
- 非同期処理の呼び出しにはawait、定義にはasyncを前につける
- TSでは非同期処理はPromise型として表現される
- ジェネリクスでは型定義に使われる型を引数として後から渡すことができる

- 時間がかかる処理
```ts
// fetchは非同期関数
const res = await fetch("http://www");
```
- 非同期関数の呼び出しを伴う関数を宣言する際には、asyncキーワードを前につけることで、新たらな非同期関数を定義できる。

```ts
async function fetchResText(url: string) {
  const res = await fetch(url);
  const html = await res.text();

  // htmlはstring型
  return html;
}
```
- 戻り値の型をつける場合は
- Promise<T> の T は「どんな型でもOK」なジェネリクス！
```ts
async function fetchResText(url: string): Promise<string>
```
