# CLAUDE.md

このファイルは、このリポジトリでコードを扱う際の Claude Code (claude.ai/code) へのガイダンスを提供します。

## プロジェクト: [PROJECT_NAME] CLI

## 開発コマンド

- 依存関係のインストール: `pip install -e .` または `poetry install`
- CLI の実行: `python -m [PACKAGE_NAME]` または `[COMMAND_NAME]`
- テストの実行: `pytest` または `python -m pytest`
- 特定テストの実行: `pytest tests/test_specific.py::TestClass::test_method`
- 型チェック: `mypy [PACKAGE_NAME]`
- Lint: `ruff check .`
- フォーマット: `black .` および `isort .`
- パッケージのビルド: `python -m build`

## アーキテクチャ

### プロジェクト構造
```
[PACKAGE_NAME]/
├── __main__.py       # python -m 実行のエントリポイント
├── cli.py           # Click/argparse コマンド定義
├── core/            # コアビジネスロジック
├── utils/           # ユーティリティ関数
└── config.py        # 設定管理
```

### コマンド構造
- `cli.py` にメイン CLI エントリポイント（[Click/argparse/typer] 使用）
- サブコマンドとしてコマンドを整理
- ルートレベルでグローバルオプションを処理
- 各関数でコマンド固有のオプション

### エラーハンドリング
- カスタム例外は `[PACKAGE_NAME]/exceptions.py` に配置
- click.echo またはコンソール出力経由のユーザーフレンドリーなエラーメッセージ
- --verbose フラグでデバッグ用の詳細モード
- 終了コードは Unix 規約に従う（成功は 0、エラーは 1-255）

### 設定
- `~/.config/[PROJECT_NAME]/config.yaml` での設定ファイルサポート
- `[PREFIX]_` プレフィックス付きの環境変数
- コマンドライン引数が設定ファイルをオーバーライド
- デフォルト値は `[PACKAGE_NAME]/config.py` に配置

### ロギング
- Python の logging モジュールを使用した構造化ロギング
- ログレベル: DEBUG, INFO, WARNING, ERROR, CRITICAL
- ログファイルの場所: `~/.local/share/[PROJECT_NAME]/logs/`
- --verbose/--quiet フラグでコンソール出力を制御

## テスト戦略

- 単体テストは `tests/unit/` に配置
- 統合テストは `tests/integration/` に配置
- フィクスチャは `tests/conftest.py` に配置
- 外部依存関係をモック
- カバレッジ目標: 最小 80%

## 主要コンポーネント

### CLI パーサー (`cli.py`)
- メインコマンドグループの定義
- オプションと引数付きのサブコマンド
- 入力検証と型変換
- ヘルプテキストと使用例

### コアロジック (`core/`)
- CLI インターフェースから分離されたビジネスロジック
- 可能な限り純粋関数
- テスト可能性のための依存性注入

### ユーティリティ (`utils/`)
- ファイル I/O 操作
- データ検証関数
- 共通フォーマットユーティリティ
- 外部 API 統合

## 一般的な開発タスク

### 新しいコマンドの追加
1. `cli.py` にコマンド関数を追加
2. `core/` モジュールにロジックを実装
3. `tests/test_commands.py` にテストを追加
4. ヘルプテキストとドキュメントを更新

### 設定オプションの追加
1. `config.py` の設定スキーマに追加
2. 設定ファイルテンプレートを更新
3. 環境変数マッピングを追加
4. README に文書化

### 外部依存関係の処理
1. `pyproject.toml` または `requirements.txt` に追加
2. 必要に応じて `utils/` にラッパーを作成
3. `unittest.mock` を使用してテストでモック
4. インポートエラーを優雅に処理

## ビルドと配布

### パッケージのビルド
```bash
# 配布パッケージのビルド
python -m build

# PyPI へのアップロード（認証情報が必要）
python -m twine upload dist/*
```

### インストール方法
- PyPI から: `pip install [PACKAGE_NAME]`
- ソースから: `pip install -e .`
- pipx 使用: `pipx install [PACKAGE_NAME]`

## パフォーマンスの考慮事項

- 高価なモジュールの遅延インポート
- 大規模データ処理にジェネレーターを使用
- 高価な計算をキャッシュ
- 並行操作のための非同期 I/O

## エラーコード

- 0: 成功
- 1: 一般的なエラー
- 2: 無効な引数
- 3: ファイルが見つからない
- 4: 権限が拒否された
- 5: 設定エラー
- [追加のプロジェクト固有のコード]