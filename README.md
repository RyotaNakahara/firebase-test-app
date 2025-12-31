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

### トラブルシューティング

#### `Error: Input required and not supplied: firebaseServiceAccount` エラーが出る場合

このエラーは、GitHub Secretsに`FIREBASE_SERVICE_ACCOUNT`が設定されていないことを示しています。

**解決方法：**

1. GitHubリポジトリの **Settings** → **Secrets and variables** → **Actions** に移動
2. **Repository secrets** セクションで `FIREBASE_SERVICE_ACCOUNT` が存在するか確認
3. 存在しない場合、または値が空の場合は、以下の手順で設定：
   - **New repository secret** をクリック
   - Name: `FIREBASE_SERVICE_ACCOUNT` （正確にこの名前で）
   - Secret: Firebase サービスアカウントの JSON キー全体を貼り付け
   - **Add secret** をクリック

**注意点：**
- シークレット名は大文字・小文字を区別します（`FIREBASE_SERVICE_ACCOUNT` が正しい）
- JSONファイルの内容全体（`{` から `}` まで）をコピーしてください
- 改行や空白も含めて、そのまま貼り付けてください

#### PRマージ時にデプロイされない場合

1. **GitHub Secretsの確認**
   - `FIREBASE_SERVICE_ACCOUNT` が正しく設定されているか確認
   - シークレット名は正確に `FIREBASE_SERVICE_ACCOUNT` である必要があります

2. **GitHub Actionsの実行ログを確認**
   - リポジトリの **Actions** タブでワークフローの実行状況を確認
   - エラーメッセージを確認してください

3. **手動実行でテスト**
   - **Actions** タブで `Deploy to Firebase Hosting` ワークフローを選択
   - **Run workflow** ボタンから手動で実行できるか確認

4. **ブランチ名の確認**
   - デフォルトブランチが `main` であることを確認
   - 異なる場合は、`.github/workflows/firebase-deploy.yml` のブランチ名を修正

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
