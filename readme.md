# Webコース学習プラットフォーム

## 概要
Webコースで行っている訓練を整理し、学習プラットフォームにまとめる。
利点は以下の通り。

- 環境を選ばずに訓練を行うことができる
- 身につけたスキルを自身で確認できる
- スキルを自己紹介状に書くことができる
- カリキュラムの見通しがわかる
- 訓練を自己管理できる

## 仕様
### ユーザ認証
- google認証を使用して認証を行う。
- "nvrcd.ac.jp"ドメイン以外のメールアドレスの登録は許可しない。
- ユーザーネームはgoogleから提供されたユーザー名を使用する。

### 講座
- 訓練モジュールの中には複数の単元があり、単元の中には複数の講座がある。  
  モジュール番号は訓練モジュールと紐づいている。

> 例）  
モジュール名：スクリプト言語演習  
モジュール番号：P-531  
単元名：JavaScript基礎  
講座名：配列の操作（pushとpop）  

- 各講座には必ず練習問題が存在し、各単元には総合問題が存在する。
- 練習問題をクリア（指導員からのOKをもらうこと）しないと次の講座には進めない。  
  提出された課題を指導員がすぐに確認できないこともありえるので、何講座か先までは開放するようにする？**要検討**
- 総合問題をクリアしないと次の単元には進むことができない。
- mdファイルをアップロードする方式で実装する？**要検討**
- youtubeの動画を埋め込むことができる。
- codepenやstackblitzを埋め込むことができる。
- 各練習問題、総合問題をクリアすることでアチーブメント（できるようになったこと）が増えていく。
- 下書き状態のものは訓練生に表示させない。

### ロール
Laravel Permissionを使うことで実装のハードルが下がる？

#### 指導員
- 自分が作成したモジュール、単元、講座を作成、編集、削除することができる。
- すべての訓練生の進捗状況を確認することができる。

#### 訓練生
- 自分が取り組んだモジュール、単元、講座の進捗状況を確認することができる。
- 自分のアチーブメントを確認することができる。
- 今まで行ってきた質問と回答を確認することができる。**要検討**

## データベース構成
### usersテーブル
- user_id (primary)
- user_name
- email
- google_id　グーグル認証から提供されるユーザーID
- google_profile　グーグル認証から渡されるプロファイル
- role_id (foreign key)　指導員、訓練生のロール
- course_id (foreign key)　訓練コース

### rolesテーブル
- role_id (primary)
- role_name　指導員とか訓練生とか

### coursesテーブル
- course_id (primary)
- training_course　コース名　WebコースとかDTPコースとか

### progressionsテーブル
- progress_id (primary)
- user_id　外部キー
- lesson_id　外部キー
- is_completed　講座が完了しているか

### lessonsテーブル
- lesson_id (primary)
- lesson_title　講座名　配列の基礎とか
- created_at　作成日
- updated_at　修正日
- unit_id (foreign key)
- description　講座の概要
- content　講座の内容（mdファイルを追加する？）
- difficulty　講座の難易度
- category　カテゴリー htmlとかJavaScriptとかデザインとか
- achievement_id (foreign key)　講座でできるようになったこと
- is_draft　下書き状態かどうかを管理

### achievementsテーブル
- achievement_id (primary key)
- achievement_name

### unitsテーブル
- unit_id (primary)
- module_id (foreign key)
- unit_name　単元名　JavaScript初級編とか
- unit_achievement　単元でできるようになったこと

### modulesテーブル
- module_id (primary)
- module_number　モジュール番号　P-531とか
- module_title　モジュール名　スクリプト言語演習とか