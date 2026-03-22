# 学習ログ

## 2026-03-22（Phase4 設定差の確認）

- 日付:
  - `日時: 2026-03-22`
  - `フェーズ: Phase4`
  - `目的: .gitmodules の branch / update 設定と git submodule sync の反映範囲を観察する`
  - `ステータス: 進行中`
  - `実行コマンド:`
    - `git config -f .gitmodules --get-regexp '^submodule\\..*\\.\\(branch\\|update\\)$'`
    - `git config --get-regexp '^submodule\\..*\\.\\(branch\\|update\\)$'`
    - `git config --get-regexp '^submodule\\.'`
    - `git config -f .gitmodules submodule.submodules/child.branch main`
    - `git config -f .gitmodules --get-regexp '^submodule\\..*'`
    - `git config --get-regexp '^submodule\\..*\\.branch$'`
    - `git submodule sync -- submodules/child`
    - `git config --get-regexp '^submodule\\..*\\.branch$'`
    - `git config -f .gitmodules submodule.submodules/child.update merge`
    - `git config -f .gitmodules --get-regexp '^submodule\\..*'`
    - `git status --short`
    - `git diff -- .gitmodules`
    - `git config --get-regexp '^submodule\\..*\\.update$'`
    - `git submodule status`
  - `観測結果:`
    - `成功: .gitmodules と .git/config のどちらにも branch / update が未設定の初期状態を確認`
    - `成功: ローカル設定には submodule.submodules/child.active true と url のみがあり、branch / update は存在しないことを確認`
    - `成功: .gitmodules に branch = main を追加しても、.git/config の branch 設定には即時反映されないことを確認`
    - `成功: git submodule sync -- submodules/child は "Synchronizing submodule url" を表示し、branch は .git/config に現れないことを確認`
    - `成功: .gitmodules に update = merge を追加し、git diff で branch = main / update = merge の2行追加を確認`
    - `成功: .git/config に update も現れず、今回の設定変更は .gitmodules の差分としてのみ存在することを確認`
    - `成功: git submodule status は clean のままで、設定変更は submodule のコミット参照を変えないことを確認`
  - `現状: .gitmodules のみ変更中（M .gitmodules）。submodule 実体と gitlink は 44ddf94 のままで clean`
  - `達成: branch は追従先、update は更新方法、sync は主に URL 同期という切り分けを確認した`
  - `次: branch / update をコミットするか、あるいはいったん戻して別設定値を比較するかを決める`
  - `次アクション: 次回は git add .gitmodules から進めて設定変更をコミットするか、git restore .gitmodules で比較実験を続けるか判断する`

## 2026-03-22（Phase4 追従運用確認）

- 日付:
  - `日時: 2026-03-22`
  - `フェーズ: Phase4`
  - `目的: submodule の detached HEAD と git submodule update --remote の挙動を観察し、親側の gitlink 更新まで完了する`
  - `ステータス: 完了`
  - `実行コマンド:`
    - `git status`
    - `git log --oneline -n 8`
    - `git submodule status`
    - `git -C submodules/child branch --show-current`
    - `git -C submodules/child symbolic-ref -q HEAD`
    - `git -C submodules/child rev-parse --short HEAD`
    - `git config -f .gitmodules --get-regexp '^submodule\..*'`
    - `git config --get-regexp '^submodule\..*\.branch$'`
    - `git -C submodules/child symbolic-ref refs/remotes/origin/HEAD`
    - `git -C submodules/child log --oneline --decorate -1 origin/main`
    - `cd ~/project/shinyak/git-submodule-practice-child`
    - `git status`
    - `cat child.txt`
    - `echo "phase4 remote update test" >> child.txt`
    - `git diff`
    - `git add child.txt`
    - `git commit -m "phase4 remote update test"`
    - `git push origin main`
    - `cd ~/project/shinyak/git-submodule-practice`
    - `git -C submodules/child rev-parse --short HEAD`
    - `git -C submodules/child fetch origin`
    - `git -C submodules/child log --oneline --decorate -1 origin/main`
    - `git submodule update --remote submodules/child`
    - `git status`
    - `git submodule status`
    - `git add submodules/child`
    - `git commit -m "update submodule child to phase4 test commit"`
    - `git log --oneline -n 1`
    - `git status`
    - `git push origin main`
  - `観測結果:`
    - `成功: submodules/child は branch --show-current と symbolic-ref -q HEAD が空で、detached HEAD 状態を確認`
    - `成功: .gitmodules と .git/config に submodule.<name>.branch 未設定であることを確認`
    - `成功: submodule 側の origin/HEAD は refs/remotes/origin/main を指していることを確認`
    - `成功: 子リポジトリで 44ddf94 phase4 remote update test を作成し origin/main へ push`
    - `成功: 親配下の submodule は update --remote 前は 0027793、fetch 後の origin/main は 44ddf94 を指すことを確認`
    - `成功: git submodule update --remote submodules/child 実行後、submodule 実体が 44ddf94 へ更新`
    - `成功: 親の git status で modified: submodules/child (new commits)、git submodule status で +44ddf94... を確認`
    - `成功: 親で git add submodules/child と commit を行い、git submodule status が clean 表示へ戻ることを確認`
    - `成功: 親の dd374fb update submodule child to phase4 test commit を push して origin/main と同期`
  - `現状: Phase4 の update --remote 基本観察は完了。親・子ともに main が origin と同期し、submodule status は clean`
  - `達成: detached HEAD / remote追従 / +表示 / gitlink更新コミットのつながりを実地で確認した`
  - `次: Phase4 継続として submodule.<name>.branch と submodule.<name>.update の設定差を検証する`
  - `次アクション: 次回は git config -f .gitmodules --get-regexp '^submodule\\..*\\.(branch|update)$' から再開`

## 2026-03-21（本日ここまで）

- 日付:
  - `日時: 2026-03-21`
  - `フェーズ: Phase3`
  - `目的: 子リポジトリ更新後の追従（+表示→親更新）を完了し、Phase4へ移る前提条件を整える`
  - `ステータス: 再確認必要`
  - `実行コマンド:`
    - `git status --short -b`
    - `git log --oneline -n 8`
    - `git submodule status`
    - `git -C submodules/child log -1 --oneline`
    - `git -C submodules/child rev-parse --short HEAD`
    - `git rev-parse --short HEAD`
    - `git submodule deinit -f submodules/child`
    - `git submodule update --init --recursive submodules/child`
    - `cd ~/project/shinyak/git-submodule-practice-child && echo "updated child content" >> child.txt && git add child.txt && git commit -m "update child content"`
    - `git -C ~/project/shinyak/git-submodule-practice-child push origin main`
    - `git -C submodules/child fetch --all --prune && git -C submodules/child checkout origin/main`
    - `git add submodules/child`
    - `git ci -m"update submodule child"`
  - `観測結果:`
    - `成功: git submodule status でdeinit後の - 表示と再取得後の通常表示を確認`
    - `成功: 子側で 0027793 update child content コミットを作成`
    - `成功: 子リポジトリを push して origin へ反映`
    - `成功: git submodule status が +0027793... (remotes/origin/HEAD) を表示（親参照との差分を検知）`
    - `成功: 親で submodules/child をステージし、update submodule child をコミット`
    - `成功: git log --oneline -n 5 --submodule で 683b238 update submodule child を確認`
  - `現状: Phase3 の主要ループ（+検知→parent更新コミット）完了。次アクションの branch --show-current は未実行`
  - `達成: 子更新→push→親追従の一連操作を実体験`
  - `次: Phase4 準備（branch --show-current → git submodule update --remote の挙動確認）`
  - `次アクション: 次回は git -C ~/project/shinyak/git-submodule-practice-child branch --show-current から再開`

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
