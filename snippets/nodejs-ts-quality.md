# Node.js/TypeScript コード品質管理

Node.js/TypeScriptプロジェクトでのパッケージ管理とコード品質維持のためのルールです。

## 使用する場面
- Node.js/TypeScriptプロジェクト
- ESLintを使用したコード品質管理
- npmによるパッケージ管理を行うプロジェクト
- 厳格なコード品質基準を維持したいチーム開発

---

## CLAUDE.mdへの追加内容

```markdown
## Node.js/TypeScript開発ルール

### パッケージ管理
- **package.jsonへの依存関係追加は必ずnpmコマンドを使用**
- 直接package.jsonを編集しない

    # 通常の依存関係
    npm install <package-name>
    
    # 開発依存関係
    npm install --save-dev <package-name>
    
    # 特定バージョンのインストール
    npm install <package-name>@<version>

### コード品質ルール

#### コメントによる警告抑制の禁止
- **ESLint/TypeScriptの警告を無効化するコメントは原則禁止**
- `// eslint-disable-next-line`、`// @ts-ignore`、`// @ts-nocheck`の使用を禁止
- 警告が出た場合は根本的な問題解決を行う
- やむを得ない場合は、理由を明確に記載し、チームレビューを経る

#### ESLint設定ポリシー
- **ESLint設定を緩める変更は禁止**
- 現実的な範囲でのルール追加・厳格化は推奨
- プロジェクトの品質向上につながる設定変更は積極的に採用
- ルール変更時は理由を明確にドキュメント化

**現在の厳格化設定**:

    // TypeScript厳格化
    '@typescript-eslint/explicit-function-return-type': 'error'
    '@typescript-eslint/no-non-null-assertion': 'error'
    '@typescript-eslint/prefer-nullish-coalescing': 'error'
    '@typescript-eslint/prefer-optional-chain': 'error'
    
    // コード品質ルール
    'complexity': ['error', { max: 10 }]
    'max-depth': ['error', 4]
    'max-lines-per-function': ['warn', { max: 80 }]
    'max-params': ['error', 4]
    
    // セキュリティルール
    'no-eval': 'error'
    'no-implied-eval': 'error'
    'no-new-func': 'error'
    
    // 命名規則
    '@typescript-eslint/naming-convention': [
      { selector: 'interface', format: ['PascalCase'] },
      { selector: 'class', format: ['PascalCase'] },
      { selector: 'function', format: ['camelCase'] }
    ]

**設定変更例**:

    # 許可される変更例（厳格化）
    - ルールの追加: @typescript-eslint/no-unused-vars: "error"
    - 既存ルールの厳格化: complexity: ["error", { "max": 8 }]
    
    # 禁止される変更例（緩和）
    - ルールの無効化: "no-console": "off"
    - エラーレベルの引き下げ: "@typescript-eslint/no-any": "warn" → "off"

#### 定期的なコード品質チェック
開発中は定期的に以下のフルチェックを実行すること：

    # フルコード品質チェック（推奨実行順序）
    npm run lint           # ESLint実行
    npm run format:check   # Prettierフォーマットチェック
    npm run build          # TypeScript型チェック + ビルド
    npm test               # テスト実行
    
    # または一括実行用のエイリアス
    npm run quality:check  # 全品質チェックを順次実行（要実装）

**チェック実行タイミング**:
- コミット前（Huskyで自動実行）
- プルリクエスト作成前
- 開発セッション終了時
- 大きな機能実装後
- 週次レビュー時

**エラー対応方針**:
- ESLintエラー: 必ず修正してからコミット
- TypeScriptエラー: 型安全性を優先して修正
- テストエラー: 機能が正常動作することを確認後修正
- Prettierエラー: `npm run format`で自動修正
```

## カスタマイズポイント

1. **ESLintルール**: プロジェクト固有のルールを追加
2. **チェックコマンド**: package.jsonのscriptsに合わせて調整
3. **エラーレベル**: チームの合意に基づいて調整（ただし緩和は避ける）
4. **自動化ツール**: Husky、lint-staged等の設定を追加