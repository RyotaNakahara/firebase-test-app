# firebase-test-app

Firebase Hosting を使用したテストアプリケーション

## 自動デプロイ

このプロジェクトは GitHub Actions を使用して、`main` ブランチへのマージ時に自動的に Firebase Hosting にデプロイされます。

## セットアップ

### GitHub Secrets の設定

GitHub Actions で Firebase にデプロイするには、以下のシークレットを設定する必要があります：

1. GitHub リポジトリの **Settings** → **Secrets and variables** → **Actions** に移動
2. **New repository secret** をクリック
3. 以下のシークレットを追加：

#### `FIREBASE_SERVICE_ACCOUNT`

Firebase サービスアカウントの JSON キーを設定します。

1. [Firebase Console](https://console.firebase.google.com/) にログイン
2. プロジェクト設定（⚙️）→ **サービスアカウント** タブ
3. **新しい秘密鍵の生成** をクリック
4. ダウンロードした JSON ファイルの内容全体をコピー
5. GitHub Secrets の `FIREBASE_SERVICE_ACCOUNT` の値として貼り付け

**重要**: JSON ファイルの内容全体（`{` から `}` まで）をそのままコピーしてください。

## ローカル開発

```bash
# Firebase CLI をインストール
npm install -g firebase-tools

# Firebase にログイン
firebase login

# ローカルサーバーを起動
firebase serve

# デプロイ
firebase deploy
```
