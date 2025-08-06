# Windows開発環境設定

Windows環境での開発時に必要な設定と注意事項をまとめたスニペットです。CLAUDE.mdにコピー&ペーストして使用してください。

## 使用する場面
- Windows環境で開発を行うプロジェクト
- 日本語を含むコードベース
- クロスプラットフォーム対応が必要な場合

---

## CLAUDE.mdへの追加内容

```markdown
## Windows開発環境の注意事項

### 基本原則
- **PowerShell**: `powershell`ではなく`pwsh`（PowerShell Core）を使用し、常に`-Encoding UTF8`を指定
- **パス処理**: ハードコーディングされたパス区切り文字（`\`や`/`）を避け、言語固有のパス処理ライブラリを使用
- **エンコーディング**: すべてのファイルをUTF-8（BOMなし）で統一
- **ビルドツール**: Unix固有コマンドの代わりにクロスプラットフォームツール（rimraf、cross-env等）を使用

### Git設定（必須）
```bash
git config core.autocrlf true      # 改行コード自動変換
git config core.quotepath false    # 日本語ファイル名対応
```

### よくある問題への対処

| 症状 | 対処法 |
|------|--------|
| 日本語の文字化け | PowerShell Coreと`-Encoding UTF8`を使用 |
| パスが見つからない | 言語固有のパス結合メソッドを使用 |
| npm scriptsエラー | rimraf（rm代替）、cross-env（環境変数）等を使用 |
| ファイルパス長エラー | Windows Long Path Supportを有効化 |

### 重要な注意点
- Windows環境では常にクロスプラットフォーム対応を意識してコードを書く
- ファイル操作時は必ずエンコーディングを明示的に指定
- シェルコマンドはWindows/Unix両対応のものを選択
- 言語やフレームワーク固有の詳細は、その都度適切な方法を判断して実装
```