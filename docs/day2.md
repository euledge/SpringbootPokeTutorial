# Day 2: バックエンドロジック (PokeAPI連携)

## 1. 今日の作業の概要
本日は、外部API (PokeAPI) からデータを取得し、アプリ独自の形式に加工してフロントエンドに返すバックエンドロジックを実装します。
実際の開発現場でも頻出する「外部サービス連携」のパターンを習得します。

## 2. 今日の学習ポイント
- **RestClient / RestTemplate**: Spring Bootから外部のHTTP APIを呼び出す方法を学ぶ。
- **DTO (Data Transfer Object)**: 外部APIのデータ構造と、アプリ内で扱うデータ構造を切り分けて設計する重要性を学ぶ。
- **Service Layer**: コントローラーからビジネスロジックを分離し、再利用性とテスト容易性を高める設計（レイヤードアーキテクチャ）を学ぶ。
- **JSON Processing**: JSONデータをJavaオブジェクトにマッピングする仕組み (Jackson) を理解する。

### 参考資料 (References)
- [Spring Framework: REST Clients (RestClient)](https://docs.spring.io/spring-framework/reference/integration/rest-clients.html)
- [PokeAPI Documentation](https://pokeapi.co/docs/v2)
- [Jackson Project](https://github.com/FasterXML/jackson)


## 3. タスクリスト
- [ ] PokeAPIの仕様確認 (ブラウザで叩いてみる)
- [ ] 外部APIレスポンス用DTOクラスの作成
- [ ] アプリ内部用DTOクラス (`Card`) の作成
- [ ] PokeAPIを呼び出す `PokeApiService` の実装
- [ ] パック開封ロジックの実装 (ランダムなID生成)
- [ ] `/api/pack/open` エンドポイントの作成と動作確認

## 4. 作業手順の詳細

### Step 1: PokeAPIの確認
1. ブラウザで `https://pokeapi.co/api/v2/pokemon/25` (ピカチュウ) にアクセスしてみます。
2. 膨大なJSONデータの中から、今回必要な情報（`name`, `sprites.front_default`, `types`）がどこにあるか確認します。

### Step 2: DTOクラスの作成
1. `com.example.poketutorial.dto` パッケージを作成します。
2. PokeAPIのレスポンスを受け取るためのクラス `PokemonResponse.java` を作成します。
   - ネストしたJSON構造に合わせて、内部クラスなどを定義します。
3. フロントエンドに返すためのシンプルなクラス `Card.java` を作成します。
   ```java
   public record Card(Long id, String name, String imageUrl, String type) {}
   ```

### Step 3: Serviceの実装
1. `com.example.poketutorial.service` パッケージを作成します。
2. `PokeApiService.java` を作成し、`RestClient` を使ってAPIを叩くメソッドを実装します。
   ```java
   @Service
   public class PokeApiService {
       private final RestClient restClient = RestClient.create();

       public Card getPokemonCard(int id) {
           // PokeAPIを叩いて PokemonResponse を取得
           // Card オブジェクトに変換して返す
       }
   }
   ```

### Step 4: パック開封ロジック
1. `PackService.java` を作成します。
2. `openPack()` メソッド内で、1〜1000の間でランダムな数値を5つ生成します。
3. それぞれのIDについて `PokeApiService` を呼び出し、`List<Card>` を作成して返します。

### Step 5: Controllerの作成
1. `PackController.java` を作成します。
2. `/api/pack/open` エンドポイントを定義し、`PackService` を呼び出して結果を返します。
3. ブラウザでアクセスし、ランダムな5匹のデータがJSONで返ってくることを確認します。

## 5. 理解度テスト
1. **Q: DTOを使うメリットは何ですか？**
   - (ヒント: 外部APIの変更影響、必要なデータのみへの絞り込み)
2. **Q: `Service` クラスに `@Service` アノテーションを付ける理由は何ですか？**
   - (ヒント: DIコンテナ、Springの管理下)
3. **Q: 同期処理で5回APIを叩くと遅くなる可能性があります。改善案はありますか？**
   - (ヒント: 並列処理、CompletableFuture)
4. **Q: HTTPメソッドの `GET` と `POST` の主な違いは何ですか？**
   - (ヒント: データの取得か、登録か)
5. **Q: コンストラクタインジェクション（DI）を使うメリットは何ですか？**
   - (ヒント: テストのしやすさ、不変性)

---
[Day 3へ進む](./day3.md)
