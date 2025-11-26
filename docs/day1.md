# Day 1: 環境構築とHello World

## 1. 今日の作業の概要
本日は、アプリケーション開発の土台となる「開発環境の構築」を行います。
Spring Boot (Backend) と Vue.js (Frontend) のプロジェクトをそれぞれ作成し、それらが相互に通信できる状態（Hello World）を目指します。

## 2. 今日の学習ポイント
- **Spring Initializr**: Spring Bootプロジェクトの標準的な作成方法を学ぶ。
- **Vite**: 最新のフロントエンドビルドツールを使ったVue.jsプロジェクトの作成方法を学ぶ。
- **REST API**: バックエンドがJSONデータを返す仕組みの基礎を理解する。
- **CORS (Cross-Origin Resource Sharing)**: 異なるオリジン（ポート番号が違うなど）間で通信する際のセキュリティ制約と、その設定方法を学ぶ。

### 参考資料 (References)
- [Spring Initializr](https://start.spring.io/)
- [Vite 公式ガイド](https://ja.vitejs.dev/guide/)
- [Spring Boot: Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
- [MDN: CORS (Cross-Origin Resource Sharing)](https://developer.mozilla.org/ja/docs/Web/HTTP/CORS)


## 3. タスクリスト
- [ ] Java (JDK 21) と Node.js のインストール確認
- [ ] Spring Bootプロジェクトの作成 (Spring Initializr)
- [ ] Vue.jsプロジェクトの作成 (npm create vue@latest)
- [ ] Backend: Hello World APIの実装 (`/api/hello`)
- [ ] Frontend: API呼び出しの実装と表示
- [ ] CORS設定による通信エラーの解消

## 4. 作業手順の詳細

### Step 1: Spring Bootプロジェクトの作成
1. ブラウザで [Spring Initializr](https://start.spring.io/) にアクセスします。
2. 以下の設定でプロジェクトを生成（Generate）し、ダウンロード・解凍します。
   - **Project**: Gradle - Groovy
   - **Language**: Java
   - **Spring Boot**: 3.3.x
   - **Group**: `com.example`
   - **Artifact**: `poketutorial`
   - **Packaging**: Jar
   - **Java**: 21
   - **Dependencies**: Spring Web, Spring Boot DevTools
3. 解凍したフォルダをIDE（VS CodeやIntelliJ）で開きます。

### Step 2: Hello World APIの実装
1. `src/main/java/com/example/poketutorial` 配下に `controller` パッケージを作成します。
2. `HelloController.java` を作成し、以下のコードを記述します。
   ```java
   package com.example.poketutorial.controller;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class HelloController {
       @GetMapping("/api/hello")
       public String hello() {
           return "Hello from Spring Boot!";
       }
   }
   ```
3. アプリケーションを実行し、ブラウザで `http://localhost:8080/api/hello` にアクセスして文字列が表示されるか確認します。

### Step 3: Vue.jsプロジェクトの作成
1. ターミナルでプロジェクトルート（`poketutorial`フォルダと同じ階層）を開きます。
2. コマンドを実行: `npm create vue@latest`
   - Project name: `frontend`
   - その他のオプションはすべて `No` でOK（今回はシンプルにするため）。
3. `cd frontend` -> `npm install` -> `npm run dev` を実行。
4. `http://localhost:5173` でVueの初期画面が表示されることを確認します。

### Step 4: API呼び出しとCORS設定
1. `frontend/src/App.vue` を編集し、スクリプトタグ内で `fetch` を使ってAPIを呼び出します。
   ```javascript
   <script setup>
   import { ref, onMounted } from 'vue'

   const message = ref('')

   onMounted(async () => {
     const res = await fetch('http://localhost:8080/api/hello')
     message.value = await res.text()
   })
   </script>

   <template>
     <h1>{{ message }}</h1>
   </template>
   ```
2. ブラウザのコンソールを確認すると、CORSエラーが出ているはずです。
3. Backendに戻り、`HelloController` に `@CrossOrigin` アノテーションを追加して許可します。
   ```java
   @CrossOrigin(origins = "http://localhost:5173")
   @GetMapping("/api/hello")
   ...
   ```
4. 再度確認し、画面に "Hello from Spring Boot!" と表示されれば成功です！

## 5. 理解度テスト
以下の質問に答えて、今日の学習内容を振り返りましょう。

1. **Q: `@RestController` と `@Controller` の違いは何ですか？**
   - (ヒント: レスポンスの中身がHTMLかデータか)
2. **Q: CORSエラーはなぜ発生するのですか？**
   - (ヒント: ブラウザのセキュリティポリシー)
3. **Q: Vue.jsの `onMounted` はどのようなタイミングで実行されますか？**
   - (ヒント: ライフサイクル)
4. **Q: `package.json` ファイルにはどのような情報が記述されていますか？**
   - (ヒント: 依存ライブラリ、スクリプト)
5. **Q: Spring Bootアプリケーションのエントリーポイントとなるメソッドは何ですか？**
   - (ヒント: `public static void ...`)

---
[Day 2へ進む](./day2.md)
