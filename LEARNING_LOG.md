# 学習ログ

## 2026-03-21（Phase2 進行）

- 日付:
  - `日時: 2026-03-21`
  - `フェーズ: Phase2`
  - `目的: GitHub 経由でサブモジュールを親リポジトリへ追加し、差分をコミット可能な状態にする`
  - `ステータス: 完了`
  - `実行コマンド:`
    - `git submodule add git@github.com:shinyak/git-submodule-practice-child.git submodules/child`
    - `git submodule status`
    - `git status --short`
    - `git diff --cached`
    - `git commit -m "add submodule: git-submodule-practice-child"`
    - `git status --short`
  - `観測結果:`
    - `成功: submodule status が ec7280f... submodules/child (heads/main) を表示`
    - `成功: .gitmodules の追加と gitlink(160000) のステージング差分を確認`
    - `成功: ステータスは最終的にクリーン（`git status --short` が空）`
  - `現状: submodule add により親に子リポジトリ参照が確立`
  - `達成: `.gitmodules` と gitlink の追加内容を理解した`
  - `次: `git submodule init` / `git submodule update` の違い検証へ`
  - `次アクション: 初期化・取得フローの確認を1コマンドずつ進める`

## 2026-03-15（再開準備・本日終了）

- 日付:
  - `日時: 2026-03-15`
  - `目的: 次回セッション再開時に迷わないための記録を残す`
  - `ステータス: 再確認必要`
  - `実行コマンド:`
    - `apply_patch（AGENTS.md / LEARNING_LOG.md の更新）`
  - `観測結果:`
    - `AGENTS.md に再開時ゲート（git status / git log / git submodule status 判定）を追加`
    - `LEARNING_LOG.md に未着手起点の初期エントリを追加し、次回再開位置を固定`
  - `次アクション:`
    - `次回: git status / git log --oneline -n 8 / git submodule status を実行`
    - `submodule status の結果で Phase2 / Phase3 / Phase5 へ分岐して再開`

## 2026-03-15（再開導線整備）

- 日付:
  - `日時: 2026-03-15`
  - `目的: AGENTS再開導線を整えて、学習を未着手状態で再開する`
  - `ステータス: 未着手`
  - `実行コマンド:`
    - `なし（準備のみ）`
  - `観測結果:`
    - `AGENTS.md を再開判定付きで更新。次セッションは再開ゲート（status/log/submodule status）→Phase2/3/5判定で開始`
  - `次アクション:`
    - `初回セッション開始時に 3 コマンドの状態確認を実行し、判定結果を LEARNING_LOG に記録してから Phase を決定`

## 2026-03-15（AGENTS整備）

- 日付:
  - `日時: 2026-03-15`
  - `目的: git submodule 学習運用ルール（AGENTS）の整備`
  - `ステータス: 完了`
  - `実行コマンド:`
    - `cat AGENTS.md`
    - `cat README.md`
    - `cat .gitignore`
    - `rg -n "おねえさん|お姉さん|姉|この方針|おねえ" AGENTS.md`
    - `apply_patch`（複数回）
  - `観測結果:`
    - `AGENTS.md` を新規作成し、Phase構成・ハンズオン進行・進捗更新・再開引継ぎを定義
    - `口調依存表現を「学習者／サポート側」向けの汎用表現に統一`
    - `進捗運用ルールとして「現状 / 達成 / 次」を随時明示する形式を追加`
  - `次アクション:`
    - `次回セッション再開時は本ログの最新エントリから `Phase 1: サブモジュールの基礎理解` を開始`
    - `まず git status / git submodule status を実行`
    - `学習者向けに1ステップずつハンズオンを再開`
