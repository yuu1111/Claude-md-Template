# CLAUDE.md

このファイルは、このリポジトリでコードを扱う際の Claude Code (claude.ai/code) へのガイダンスを提供します。

## プロジェクト: [PROJECT_NAME]

## ビルドコマンド

- 開発サーバー: `npm run dev` (http://localhost:3000 で実行)
- 本番ビルド: `npm run build`
- テスト実行: `npm test`
- 特定テストの実行: `npm test -- path/to/test --watch`
- 型チェック: `npm run type-check`
- Lint: `npm run lint`
- Lint 問題の修正: `npm run lint:fix`

## アーキテクチャ

### コンポーネント構造
- コンポーネントは TypeScript で関数コンポーネントを使用
- 共有ロジック用のカスタムフックは `src/hooks/` に配置
- プロップドリリングよりコンポーネントコンポジションを優先
- 各コンポーネントには対応するテストファイルがある

### 状態管理
- グローバル状態は `src/contexts/` の Context API で管理
- ローカルコンポーネント状態は useState/useReducer を使用
- サーバー状態は `src/queries/` の React Query でキャッシュ

### ルーティング
- React Router v6 の設定は `src/routes/` に配置
- 保護されたルートは `AuthGuard` コンポーネントで処理
- ルートコンポーネントの遅延ローディング

### API 統合
- すべての API 呼び出しは `src/services/api.ts` 経由
- 認証トークン注入用の Axios インターセプター
- エラーレスポンスはグローバルエラーバウンダリで処理

### スタイリング
- コンポーネントスタイルには CSS モジュール (`*.module.css`)
- グローバルスタイルは `src/styles/global.css` に配置
- デザイントークンは `src/styles/tokens.css` に配置

### テスト戦略
- Jest と React Testing Library による単体テスト
- 重要なユーザーフローの統合テスト
- API モッキングには Mock Service Worker (MSW) を使用
- カバレッジ目標: ユーティリティは 80%、コンポーネントは 70%

## プロジェクト固有のパターン

### フォーム処理
- 複雑なフォームには React Hook Form を使用
- `src/schemas/` の Zod を使用した検証スキーマ
- フォームコンポーネントは `src/components/forms/` に配置

### 認証フロー
1. ログイン認証情報を `/api/auth/login` に送信
2. JWT を httpOnly クッキーに保存
3. Auth コンテキストがユーザー状態を提供
4. 保護されたルートが認証状態をチェック

### エラーハンドリング
- API エラーは axios インターセプターでキャッチ
- UI エラーは ErrorBoundary コンポーネントでキャッチ
- `src/utils/errorMessages.ts` からのユーザーフレンドリーなエラーメッセージ

### パフォーマンス最適化
- ルートレベルでのコード分割
- Intersection Observer による画像の遅延ローディング
- 高価な計算のメモ化
- 大規模リストの仮想スクロール

## 主要ファイルとその目的

- `src/App.tsx` - メインアプリケーションコンポーネントとルート設定
- `src/contexts/AuthContext.tsx` - 認証状態とメソッド
- `src/services/api.ts` - 集中化された API 設定
- `src/components/Layout/` - アプリケーションレイアウトコンポーネント
- `src/utils/` - 共有ユーティリティ関数
- `src/types/` - TypeScript 型定義

## 開発ワークフロー

1. `main` から機能ブランチを作成
2. テストと共に機能を実装
3. `npm run lint:fix` と `npm run type-check` を実行
4. `npm test` でテストが通ることを確認
5. 変更の説明と共に PR を提出

## 環境変数

`.env` に必要：
- `REACT_APP_API_URL` - バックエンド API エンドポイント
- `REACT_APP_ENV` - 環境 (development/staging/production)

## 一般的なタスク

### 新しいページの追加
1. `src/pages/` にコンポーネントを作成
2. `src/routes/index.tsx` にルートを追加
3. 対応するテストファイルを作成
4. 必要に応じてナビゲーションを更新

### API エンドポイントの追加
1. `src/services/` の適切なサービスにメソッドを追加
2. `src/types/` に TypeScript 型を作成
3. 必要に応じて React Query フックを追加
4. エラーを適切に処理

### 再利用可能なコンポーネントの作成
1. `src/components/[ComponentName]/` にコンポーネントを配置
2. `index.tsx`、`[ComponentName].tsx`、`[ComponentName].module.css` を含める
3. `[ComponentName].test.tsx` にテストを書く
4. `src/components/index.ts` からエクスポート