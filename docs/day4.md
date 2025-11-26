# Day 4: データベース連携 - コレクション機能

## 1. 今日の作業の概要
本日は、獲得したカードデータをデータベースに保存し、永続化する機能を実装します。
これにより、ブラウザを閉じても集めたカードが消えないようになり、「収集」の楽しさが生まれます。

## 2. 今日の学習ポイント
- **Spring Data JPA**: Javaオブジェクトとデータベーステーブルをマッピングし、SQLを書かずにDB操作を行う方法を学ぶ。
- **H2 Database**: 開発・テストに便利なインメモリデータベースの使い方を学ぶ。
- **Entity**: データベースのテーブル定義に対応するクラスの書き方を学ぶ。
- **Repository Pattern**: データアクセス層を抽象化するデザインパターンを学ぶ。

### 参考資料 (References)
- [Spring Data JPA Reference](https://docs.spring.io/spring-data/jpa/reference/index.html)
- [Accessing Data with JPA (Guide)](https://spring.io/guides/gs/accessing-data-jpa/)
- [H2 Database Engine](https://www.h2database.com/html/main.html)


## 3. タスクリスト
- [ ] `application.properties` でH2 Databaseの設定
- [ ] `Card` クラスをJPA Entityに変更 (`@Entity`)
- [ ] `CardRepository` インターフェースの作成
- [ ] パック開封時にDB保存するロジックの追加
- [ ] 全カード取得API (`/api/cards`) の実装
- [ ] フロントエンド: コレクション画面の作成

## 4. 作業手順の詳細

### Step 1: データベース設定
1. `src/main/resources/application.properties` にH2の設定を追記します。
   ```properties
   spring.h2.console.enabled=true
   spring.datasource.url=jdbc:h2:mem:testdb
   spring.datasource.driverClassName=org.h2.Driver
   spring.datasource.username=sa
   spring.datasource.password=
   spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
   ```

### Step 2: EntityとRepositoryの作成
1. Day 2で作った `Card` クラス（または新規クラス）に `@Entity` アノテーションを付けます。
   - `@Id`, `@GeneratedValue` で主キーを設定します。
2. `CardRepository` インターフェースを作成し、`JpaRepository<Card, Long>` を継承します。
   - これだけで `save`, `findAll` などのメソッドが使えるようになります。

### Step 3: 保存ロジックの実装
1. `PackService` に `CardRepository` をInjectします。
2. `openPack` メソッド内で、PokeAPIから取得したデータを `Card` Entityに変換し、`repository.saveAll(...)` で保存してから返すように変更します。

### Step 4: コレクションAPIの実装
1. `CardController` (または `CollectionController`) を作成します。
2. `/api/cards` エンドポイントで `repository.findAll()` の結果を返します。

### Step 5: コレクション画面の作成
1. Vue.js側で `CollectionView.vue` を作成します。
2. マウント時に `/api/cards` を叩き、所持カード一覧を表示します。
3. ルーティング (`vue-router`) を設定し、パック開封画面と行き来できるようにします。

## 5. 理解度テスト
1. **Q: JPAを使うと何が便利ですか？**
   - (ヒント: SQL、生産性)
2. **Q: インメモリDB (H2) の欠点は何ですか？**
   - (ヒント: アプリ再起動時のデータ)
3. **Q: `JpaRepository` を継承しただけでメソッドが使えるのはなぜですか？**
   - (ヒント: Spring Dataの仕組み、Proxy)
4. **Q: `@Transactional` アノテーションはどのような時に使いますか？**
   - (ヒント: 複数のDB操作、ロールバック)
5. **Q: `application.properties` ファイルの役割は何ですか？**
   - (ヒント: DB接続情報、ポート番号などの設定)

---
[Day 5へ進む](./day5.md)
