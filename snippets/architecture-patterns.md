# アーキテクチャパターン スニペット

## 使用方法
プロジェクトのアーキテクチャの決定とパターンを文書化するために、このセクションを追加してください。
特定のアーキテクチャに基づいてカスタマイズしてください。

---

## アーキテクチャパターン

### 状態管理
- [STATE_PATTERN]（例：「`src/store/` の Redux ストア」、「`src/contexts/` の Context API プロバイダー」、「`src/stores/` の Zustand ストア」）
- [STATE_CONVENTION]（例：「各機能には独自のスライスがある」、「グローバル状態はコンポーネント間のデータのみ」）

### データフロー
- [API_PATTERN]（例：「axios を使用した `src/services/` 経由の API 呼び出し」、「`src/graphql/` の GraphQL クエリ」）
- [DATA_FETCHING]（例：「サーバー状態には React Query を使用」、「データフェッチには SWR を使用」）
- [ERROR_HANDLING]（例：「アプリレベルでの集中エラーバウンダリ」、「すべての非同期関数で try-catch」）

### コンポーネント構造
- [COMPONENT_PATTERN]（例：「継承よりコンポジション」、「コンテナ/プレゼンターパターン」、「アトミックデザイン方法論」）
- [STYLING_APPROACH]（例：「`*.module.css` を使用した CSS モジュール」、「Styled-components」、「Tailwind ユーティリティクラス」）
- [COMPONENT_LOCATION]（例：「共有コンポーネントは `src/components/`」、「機能コンポーネントは `src/features/[feature]/components/`」）

### ファイル構成
- [ORGANIZATION_PATTERN]（例：「機能ベース構造」、「レイヤーベース構造」、「ドメイン駆動設計」）
- [NAMING_CONVENTION]（例：「コンポーネントは PascalCase」、「ユーティリティは camelCase」、「ファイルは kebab-case」）