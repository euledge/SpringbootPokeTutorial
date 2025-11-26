# Day 3: フロントエンド - パック開封UI

## 1. 今日の作業の概要
本日は、Vue.jsを使ってユーザーが実際に操作できる画面を作成します。
Day 2で作ったAPIを呼び出し、取得したカードデータをリッチに表示するコンポーネントを実装します。

## 2. 今日の学習ポイント
- **Vue Components**: 画面の部品化（`Card.vue`）と、Propsを使ったデータの受け渡しを学ぶ。
- **Reactivity**: `ref` や `reactive` を使い、データの変更に合わせて画面を自動更新する仕組みを学ぶ。
- **List Rendering**: `v-for` ディレクティブを使って、配列データをループ表示する方法を学ぶ。
- **Event Handling**: ボタンクリックなどのユーザー操作を検知し、関数を実行する方法を学ぶ。

### 参考資料 (References)
- [Vue.js: コンポーネントの基礎](https://ja.vuejs.org/guide/essentials/component-basics.html)
- [Vue.js: リアクティビティーの基礎](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html)
- [Vue.js: リストレンダリング (v-for)](https://ja.vuejs.org/guide/essentials/list.html)
- [Vue.js: イベントハンドリング](https://ja.vuejs.org/guide/essentials/event-handling.html)


## 3. タスクリスト
- [ ] `Card.vue` コンポーネントの作成
- [ ] パック開封画面 (`PackView.vue`) の作成
- [ ] 開封ボタンの実装とAPI連携
- [ ] グリッドレイアウトでのカード表示
- [ ] 簡易的なローディング表示の実装

## 4. 作業手順の詳細

### Step 1: Cardコンポーネントの作成
1. `frontend/src/components/Card.vue` を作成します。
2. Propsとして `card` オブジェクトを受け取るように定義します。
   ```javascript
   <script setup>
   defineProps(['card'])
   </script>
   ```
3. template内で、画像の `src` や名前を表示するHTMLを記述します。
4. CSSでカードらしい見た目（枠線、角丸、影など）を整えます。

### Step 2: パック開封画面の作成
1. `frontend/src/views/PackView.vue` を作成します（または `App.vue` に直接記述してもOK）。
2. 「パックを開ける」ボタンを配置します。
3. カードリストを格納するリアクティブな変数 `cards` を用意します。
   ```javascript
   const cards = ref([])
   ```

### Step 3: API連携と表示
1. ボタンの `@click` イベントで `openPack` 関数を呼び出します。
2. `openPack` 関数内で `fetch('http://localhost:8080/api/pack/open')` を実行します。
3. 取得したデータを `cards.value` に代入します。
4. template内で `v-for` を使い、`Card` コンポーネントをループ表示します。
   ```html
   <div class="card-grid">
     <Card v-for="card in cards" :key="card.id" :card="card" />
   </div>
   ```

### Step 4: スタイリング調整
1. CSS Grid または Flexbox を使い、カードが横並び（スマホなら折り返し）になるようにレイアウトします。
2. 開封中はボタンを無効化するなど、UXを少し改善します。

## 5. 理解度テスト
1. **Q: `v-for` を使う際に `:key` が必要な理由は何ですか？**
   - (ヒント: 描画パフォーマンス、DOMの再利用)
2. **Q: 親コンポーネントから子コンポーネントにデータを渡す仕組みは何ですか？**
   - (ヒント: Pから始まる言葉)
3. **Q: `ref` と `reactive` の使い分けの基準は？**
   - (ヒント: プリミティブ値かオブジェクトか、.valueが必要か)
4. **Q: `computed` プロパティ（算出プロパティ）を使うメリットは何ですか？**
   - (ヒント: キャッシュ、再計算)
5. **Q: `<style scoped>` の `scoped` 属性は何のために付けますか？**
   - (ヒント: CSSの影響範囲)

---
[Day 4へ進む](./day4.md)
