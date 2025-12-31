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

**ステップ1: Firebase Console からサービスアカウントキーを取得**

1. [Firebase Console](https://console.firebase.google.com/) にログイン
2. プロジェクト `deploy-test-app-7d412` を選択
3. 左上の⚙️（歯車アイコン）をクリック → **プロジェクト設定**
4. **サービスアカウント** タブをクリック
5. **新しい秘密鍵の生成** ボタンをクリック
6. 確認ダイアログで **キーの生成** をクリック
7. JSON ファイルがダウンロードされます

**ステップ2: JSON キーをコピー**

1. ダウンロードした JSON ファイルをテキストエディタで開く
2. **ファイル全体の内容を選択してコピー**（`Ctrl+A` / `Cmd+A` → `Ctrl+C` / `Cmd+C`）
   - ファイルの内容は以下のような形式です：
   ```json
   {
     "type": "service_account",
     "project_id": "deploy-test-app-7d412",
     "private_key_id": "...",
     "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
     ...
   }
   ```

**ステップ3: GitHub Secrets に設定**

1. GitHub リポジトリのページに移動
2. **Settings** タブをクリック
3. 左側のメニューから **Secrets and variables** → **Actions** をクリック
4. **New repository secret** ボタンをクリック
5. 以下の情報を入力：
   - **Name**: `FIREBASE_SERVICE_ACCOUNT` （正確にこの名前で、大文字小文字も正確に）
   - **Secret**: ステップ2でコピーした JSON ファイルの内容全体を貼り付け（`Ctrl+V` / `Cmd+V`）
6. **Add secret** ボタンをクリック

**重要**: 
- JSON ファイルの内容全体（最初の `{` から最後の `}` まで）をそのままコピーしてください
- 改行や空白も含めて、そのまま貼り付けてください
- シークレット名は正確に `FIREBASE_SERVICE_ACCOUNT` である必要があります（大文字小文字を区別します）

**ステップ4: 設定の確認**

シークレットが正しく設定されたことを確認：
1. **Settings** → **Secrets and variables** → **Actions** に戻る
2. **Repository secrets** セクションに `FIREBASE_SERVICE_ACCOUNT` が表示されていることを確認
3. 値は `••••••••` のようにマスクされて表示されます（これは正常です）

### トラブルシューティング

#### `Error: FIREBASE_SERVICE_ACCOUNT secret is not set` エラーが出る場合

このエラーは、GitHub Secretsに`FIREBASE_SERVICE_ACCOUNT`が設定されていないことを示しています。

**解決方法：**

上記の「`FIREBASE_SERVICE_ACCOUNT`の設定」手順に従って、シークレットを設定してください。

**設定後の確認：**

1. シークレットを設定したら、**Actions** タブに移動
2. **Deploy to Firebase Hosting** ワークフローを選択
3. **Run workflow** ボタンをクリックして手動実行
4. 実行ログで `✓ FIREBASE_SERVICE_ACCOUNT secret is configured` というメッセージが表示されれば成功です

**よくある問題：**

- **シークレット名のタイプミス**: `FIREBASE_SERVICE_ACCOUNT` と正確に入力してください（`FIREBASE_SERVICE_ACCOUNT` のスペル確認）
- **JSONの一部のみコピー**: JSONファイルの全体をコピーしてください（`{` から `}` まで）
- **スペースや改行の削除**: 改行や空白も含めて、そのまま貼り付けてください
- **別のシークレット名を使用**: `FIREBASE_SERVICE_ACCOUNT` という名前で統一してください

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
