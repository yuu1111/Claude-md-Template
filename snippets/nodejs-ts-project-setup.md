# Node.js/TypeScript プロジェクト総合設定

Node.js/TypeScriptプロジェクトの包括的な設定ガイドです。モダンな開発環境構築のベストプラクティスを提供します。

## 使用する場面
- 新規Node.js/TypeScriptプロジェクトの立ち上げ
- 既存プロジェクトのTypeScript移行
- モジュールシステムの設定
- ビルド環境の構築
- 開発環境の標準化

---

## CLAUDE.mdへの追加内容

```markdown
## Node.js/TypeScript プロジェクト設定

### TypeScript設定（tsconfig.json）

**基本構成要素**:

**コンパイラオプション - 厳格モード**:
- `strict: true` - すべての厳格な型チェックオプションを有効化
- `noImplicitAny: true` - 暗黙的なany型を禁止
- `strictNullChecks: true` - null/undefinedの厳密なチェック
- `strictFunctionTypes: true` - 関数型の厳密なチェック
- `strictBindCallApply: true` - bind/call/applyの厳密なチェック
- `strictPropertyInitialization: true` - クラスプロパティの初期化を強制
- `noImplicitThis: true` - 暗黙的なthisを禁止
- `alwaysStrict: true` - 常にstrictモードでコンパイル

**コンパイラオプション - 追加チェック**:
- `noUnusedLocals: true` - 未使用のローカル変数を検出
- `noUnusedParameters: true` - 未使用のパラメータを検出
- `noImplicitReturns: true` - すべてのコードパスでreturnを強制
- `noFallthroughCasesInSwitch: true` - switch文のフォールスルーを禁止
- `noUncheckedIndexedAccess: true` - インデックスアクセスをundefinable化
- `noImplicitOverride: true` - overrideキーワードを必須化
- `noPropertyAccessFromIndexSignature: true` - インデックスシグネチャへのドットアクセスを禁止

**モジュール解決設定**:
- `module: "NodeNext"` - Node.js用の最新モジュールシステム
- `moduleResolution: "NodeNext"` - Node.js用の解決アルゴリズム
- `esModuleInterop: true` - CommonJSとESMの相互運用性
- `allowSyntheticDefaultImports: true` - デフォルトインポートの許可
- `resolveJsonModule: true` - JSONファイルのインポート許可
- `moduleDetection: "force"` - すべてのファイルをモジュールとして扱う

**出力設定**:
- `target: "ES2022"` - 出力するJavaScriptのバージョン
- `lib: ["ES2022"]` - 使用可能なライブラリ定義
- `outDir: "./dist"` - 出力ディレクトリ
- `rootDir: "./src"` - ソースコードのルートディレクトリ
- `declaration: true` - 型定義ファイルの生成
- `declarationMap: true` - 型定義のソースマップ生成
- `sourceMap: true` - ソースマップの生成
- `removeComments: true` - コメントの削除

**パス設定**:
- `baseUrl: "./src"` - パス解決のベースURL
- `paths` - パスエイリアスの設定（例: `@/*` → `src/*`）
- `typeRoots: ["./node_modules/@types", "./types"]` - 型定義の検索場所
- `types` - 含める型定義パッケージの指定

**プロジェクト構成**:
- `include: ["src/**/*"]` - コンパイル対象のファイル
- `exclude: ["node_modules", "dist", "**/*.test.ts"]` - 除外するファイル

### package.json設定

**基本情報**:
- `type: "module"` - ESモジュールを使用（CommonJSの場合は省略）
- `engines` - Node.jsとnpmの必須バージョン指定
- `packageManager` - 使用するパッケージマネージャーの指定

**スクリプト構成**:

**開発用スクリプト**:
- `dev` - 開発サーバーの起動（ts-node-dev、tsx、nodemon等）
- `watch` - ファイル変更の監視とリビルド
- `debug` - デバッグモードでの起動

**ビルド・検証スクリプト**:
- `build` - TypeScriptのコンパイル
- `build:clean` - 出力ディレクトリのクリーンアップ後ビルド
- `type-check` - 型チェックのみ実行（ビルドなし）
- `lint` - ESLintの実行
- `format` - Prettierでのフォーマット
- `test` - テストの実行
- `test:watch` - テストのウォッチモード
- `test:coverage` - カバレッジ付きテスト

**本番用スクリプト**:
- `start` - 本番環境での起動
- `serve` - ビルド済みファイルの実行

**エクスポート設定（ライブラリの場合）**:
- `main` - CommonJS用エントリーポイント
- `module` - ESモジュール用エントリーポイント
- `types` - TypeScript型定義ファイル
- `exports` - 条件付きエクスポート設定

### モジュールシステムの選択

**ESモジュール（推奨）**:
- package.jsonに `"type": "module"` を追加
- インポート時に拡張子を明記（`.js`）
- `__dirname`と`__filename`の代替実装が必要
- 最新のNode.js機能を活用可能

**CommonJS（従来型）**:
- package.jsonの`type`フィールドは不要
- require/module.exportsを使用
- 既存のNode.jsエコシステムとの互換性高い
- 移行が容易

**ハイブリッド対応**:
- `.mjs` - ESモジュール
- `.cjs` - CommonJS
- `.js` - package.jsonのtypeに依存

### 開発ツール設定

**ts-node設定（開発実行用）**:
- `ts-node` セクションをtsconfig.jsonに追加
- `transpileOnly: true` - 型チェックをスキップして高速化
- `swc: true` - SWCコンパイラを使用して高速化
- `esm: true` - ESモジュールサポート

**tsx（代替実行ツール）**:
- ts-nodeより高速
- 設定不要で動作
- watch機能内蔵

**nodemon設定（ファイル監視）**:
- `nodemon.json`で監視対象を設定
- TypeScriptファイルの変更を検出
- 自動再起動の設定

### ビルド最適化

**開発環境**:
- インクリメンタルビルド有効化
- ソースマップ生成
- 型チェックと並列実行

**本番環境**:
- minify/圧縮
- ツリーシェイキング
- 不要なコメント削除
- ソースマップの外部化または削除

### プロジェクト構造

    project-root/
    ├── src/                 # ソースコード
    │   ├── index.ts        # エントリーポイント
    │   ├── types/          # 型定義
    │   ├── utils/          # ユーティリティ
    │   └── lib/            # ライブラリコード
    ├── dist/               # ビルド出力
    ├── tests/              # テストファイル
    ├── docs/               # ドキュメント
    ├── .github/            # GitHub設定
    ├── tsconfig.json       # TypeScript設定
    ├── tsconfig.build.json # ビルド用設定
    ├── .eslintrc.json      # ESLint設定
    ├── .prettierrc         # Prettier設定
    ├── jest.config.js      # Jest設定
    ├── package.json        # パッケージ設定
    └── README.md           # プロジェクト説明

### 依存関係管理

**本番依存関係（dependencies）**:
- 実行時に必要なパッケージのみ
- 型定義は含めない
- 最小限のセキュリティリスク

**開発依存関係（devDependencies）**:
- TypeScript、ESLint、Prettier
- テストフレームワーク
- ビルドツール
- 型定義（@types/*）

**peerDependencies（ライブラリの場合）**:
- 利用側で提供すべき依存関係
- バージョン範囲の指定

### Node.jsバージョン管理

**.nvmrc/.node-version**:
- プロジェクトで使用するNode.jsバージョンを固定
- チーム全体で同じバージョンを使用

**engines フィールド**:
- package.jsonで最小要求バージョンを指定
- npmやyarnが警告を表示

### 環境変数管理

**.env ファイル構成**:
- `.env` - デフォルト設定
- `.env.local` - ローカル設定（gitignore）
- `.env.development` - 開発環境
- `.env.production` - 本番環境
- `.env.test` - テスト環境

**dotenv設定**:
- 環境に応じた設定ファイルの読み込み
- 型安全な環境変数アクセス
- バリデーション機能

### デバッグ設定

**VS Code設定（.vscode/launch.json）**:
- TypeScriptのデバッグ設定
- ソースマップの有効化
- ブレークポイントの設定
- 環境変数の注入

**Chrome DevTools**:
- Node.js inspectorの使用
- `--inspect`フラグでデバッグポート開放
- Chrome://inspectでアタッチ

### パフォーマンス最適化

**コンパイル速度向上**:
- `incremental: true` - インクリメンタルコンパイル
- `tsBuildInfoFile` - ビルド情報のキャッシュ
- プロジェクト参照の活用
- SWCやesbuildの使用

**実行速度向上**:
- 適切なtarget設定
- 不要なポリフィル削除
- バンドルサイズの最適化

### CI/CD設定

**GitHub Actions基本設定**:
- Node.jsバージョンマトリックス
- 依存関係のキャッシュ
- 並列テスト実行
- ビルド成果物の保存

**品質チェック**:
- 型チェック
- Lintチェック
- テスト実行
- カバレッジ測定
- セキュリティ監査
```

## カスタマイズポイント

1. **プロジェクトタイプ**: アプリケーション vs ライブラリ
2. **モジュールシステム**: ESM vs CommonJS vs ハイブリッド
3. **ターゲット環境**: Node.jsバージョンに応じた設定
4. **ビルドツール**: tsc、esbuild、SWC、webpack等の選択

## 関連スニペット

- `eslint-recommended-config.md`: ESLint詳細設定
- `nodejs-ts-quality.md`: コード品質管理
- `testing-rules.md`: テスト設定
- `versioning-and-commits.md`: バージョニングルール