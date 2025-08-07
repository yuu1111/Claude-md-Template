# ESLint おすすめ設定

プロジェクトタイプ別のESLint推奨設定集です。コード品質を高める厳格な設定を提供します。

## 使用する場面
- 新規プロジェクトのESLint設定
- 既存プロジェクトのESLint設定強化
- TypeScript/JavaScript プロジェクト
- React/Vue/Node.js アプリケーション

---

## CLAUDE.mdへの追加内容

```markdown
## ESLint設定ガイドライン

### 基本設定ファイル構成

**.eslintrc.json の構成要素**:

**基本設定**:
- `root: true` - 上位ディレクトリの設定ファイルを検索しない
- 環境設定 - ES2022、Node.js、ブラウザ環境を有効化

**継承する設定（extends）**:
- ESLint推奨ルール
- TypeScript ESLint推奨ルール（型チェック含む）
- import プラグインの推奨設定
- Prettier との競合回避設定

**パーサー設定**:
- TypeScriptパーサーを使用
- ECMAScript 2022対応
- モジュール形式のソースコード
- tsconfig.jsonとの連携

**使用プラグイン**:
- TypeScript ESLint - TypeScript専用ルール
- import - インポート/エクスポート管理
- unused-imports - 未使用インポートの検出

### 推奨ルール設定

#### TypeScript厳格化ルール

**型安全性の向上**:
- `explicit-function-return-type` - 関数の戻り値型を明示的に指定
- `no-explicit-any` - any型の使用を禁止
- `no-unsafe-assignment` - 型安全でない代入を禁止
- `no-unsafe-member-access` - 型安全でないメンバーアクセスを禁止
- `no-unsafe-call` - 型安全でない関数呼び出しを禁止
- `no-unsafe-return` - 型安全でない戻り値を禁止

**Null安全性**:
- `no-non-null-assertion` - non-null assertion（!）の使用を禁止
- `prefer-nullish-coalescing` - || の代わりに ?? の使用を推奨
- `prefer-optional-chain` - オプショナルチェーンの使用を推奨
- `strict-boolean-expressions` - 厳密なboolean式のチェック

**未使用コードの検出**:
- `unused-imports/no-unused-imports` - 未使用のインポートを自動削除
- `unused-imports/no-unused-vars` - 未使用変数の検出（_で始まる変数は除外）

#### コード品質ルール

**複雑度制限**:
- `complexity` - 循環的複雑度を最大10に制限
- `max-depth` - ネストの深さを最大4レベルに制限
- `max-nested-callbacks` - コールバックのネストを最大3レベルに制限
- `max-lines` - ファイルあたり最大300行（空行・コメント除く）
- `max-lines-per-function` - 関数あたり最大50行
- `max-params` - 関数パラメータを最大4個に制限
- `max-statements` - 関数内のステートメントを最大15個に制限

**コードスタイル**:
- `curly` - すべての制御構文で中括弧を強制
- `eqeqeq` - 厳密等価演算子（===）の使用を強制
- `no-nested-ternary` - ネストした三項演算子を禁止
- `prefer-const` - 再代入しない変数はconstを使用
- `prefer-template` - 文字列連結の代わりにテンプレートリテラルを使用
- `no-var` - varの使用を禁止（let/constを使用）

**エラー防止**:
- `no-console` - console使用を警告（warn、error、infoは許可）
- `no-debugger` - debugger文を禁止
- `no-alert` - alert、confirm、promptを禁止
- `no-eval` - eval()の使用を禁止
- `no-implied-eval` - 暗黙的なeval（setTimeout等）を禁止
- `no-new-func` - Function コンストラクタを禁止

#### Import/Export管理

**インポート順序**:
- グループ順: builtin → external → internal → parent → sibling → index → object → type
- グループ間に空行を挿入
- アルファベット順（大文字小文字を区別しない）でソート

**インポート検証**:
- `import/no-duplicates` - 重複インポートを禁止
- `import/no-cycle` - 循環参照を禁止
- `import/no-self-import` - 自己インポートを禁止
- `import/no-unresolved` - 解決できないインポートを禁止
- `import/no-default-export` - デフォルトエクスポートを禁止（名前付きエクスポート推奨）

**エクスポート検証**:
- `import/no-mutable-exports` - ミュータブルなエクスポートを禁止
- `import/no-unused-modules` - 未使用のエクスポートを検出

#### 命名規則

**TypeScript要素の命名パターン**:
- **インターフェース**: PascalCase（Iプレフィックスなし）
- **型エイリアス**: PascalCase
- **クラス**: PascalCase
- **列挙型**: PascalCase
- **列挙型メンバー**: UPPER_CASE
- **変数**: camelCase（定数はUPPER_CASEも可）
- **関数**: camelCase
- **プライベートプロパティ**: _camelCase（アンダースコアプレフィックス必須）

### React専用設定（React プロジェクトの場合）

**追加の extends**:
    "extends": [
      // ... 既存の設定
      "plugin:react/recommended",
      "plugin:react-hooks/recommended",
      "plugin:jsx-a11y/recommended"
    ]

**React専用ルール**:

**React Hooks**:
- `rules-of-hooks` - Hooksの呼び出しルールを強制
- `exhaustive-deps` - useEffectの依存配列を完全にする

**React一般**:
- `prop-types` - TypeScript使用時は無効化
- `react-in-jsx-scope` - React 17以降は不要
- `jsx-no-useless-fragment` - 不要なFragmentを禁止
- `jsx-curly-brace-presence` - 不要な中括弧を削除
- `self-closing-comp` - 子要素のないコンポーネントは自己終了タグ
- `jsx-sort-props` - プロップスの並び順（予約語→短縮形→コールバック）

**アクセシビリティ**:
- `alt-text` - img要素にalt属性を必須化
- `anchor-is-valid` - 有効なアンカータグのみ許可
- `click-events-have-key-events` - クリックイベントにキーボードイベントも必須
- `no-static-element-interactions` - 静的要素への直接的なイベントハンドラを禁止

### Node.js専用設定（バックエンドの場合）

**Node.js専用ルール**:

**セキュリティ**:
- `no-process-exit` - process.exit()の直接呼び出しを禁止
- `no-process-env` - 環境変数へのアクセスは許可

**非同期処理**:
- `no-await-in-loop` - ループ内でのawaitを禁止（パフォーマンス向上）
- `require-await` - asyncキーワードを持つ関数は必ずawaitを使用
- `no-floating-promises` - Promiseの結果を必ず処理
- `no-misused-promises` - Promiseの誤用を防止
- `promise-function-async` - Promiseを返す関数はasyncを使用

**エラーハンドリング**:
- `handle-callback-err` - コールバックのエラーパラメータを必ず処理
- `no-throw-literal` - Error オブジェクト以外のthrowを禁止

### package.json Scripts設定

**推奨スクリプト**:
- `lint` - ESLintを実行して問題を検出
- `lint:fix` - ESLintで自動修正可能な問題を修正
- `lint:strict` - 警告も許可しない厳格モード
- `type-check` - TypeScriptの型チェック（ビルドなし）
- `quality:check` - lint:strictとtype-checkを連続実行

### 必要なパッケージ

**基本パッケージ**:
- eslint - ESLint本体
- @typescript-eslint/parser - TypeScriptパーサー
- @typescript-eslint/eslint-plugin - TypeScript用ルール
- eslint-plugin-import - import/export管理
- eslint-plugin-unused-imports - 未使用インポートの検出
- eslint-config-prettier - Prettierとの競合回避

**React プロジェクトの場合**:
- eslint-plugin-react - React用ルール
- eslint-plugin-react-hooks - React Hooks用ルール
- eslint-plugin-jsx-a11y - アクセシビリティチェック

### 段階的導入戦略

1. **Phase 1 - 基本設定** (1週目)
   - 基本的なESLint設定を導入
   - エラーレベルのルールのみ有効化
   - チーム全体でlint実行を習慣化

2. **Phase 2 - 型安全性強化** (2-3週目)
   - TypeScript厳格化ルールを追加
   - 既存コードの型エラーを修正
   - CI/CDにlintチェックを組み込み

3. **Phase 3 - コード品質向上** (4-5週目)
   - 複雑度制限ルールを追加
   - インポート管理ルールを有効化
   - 命名規則を統一

4. **Phase 4 - 完全適用** (6週目以降)
   - すべてのルールを有効化
   - `--max-warnings 0`で警告も許可しない
   - pre-commitフックで自動チェック
```

## カスタマイズポイント

1. **プロジェクトタイプ**: React/Vue/Node.jsに応じてプラグインを選択
2. **厳格度レベル**: チームの成熟度に応じて段階的に厳格化
3. **独自ルール**: プロジェクト固有のルールを追加
4. **除外パターン**: .eslintignoreで不要なファイルを除外

## 関連スニペット

- `nodejs-ts-quality.md`: Node.js/TypeScript品質管理の詳細
- `versioning-and-commits.md`: コミット規約との連携
- `testing-rules.md`: テストコードのESLint設定