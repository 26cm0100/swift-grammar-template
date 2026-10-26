# 読解ノート：自分のアプリのコード3箇所

> **氏名：** 安住光司
> **AI利用レベル：** 1（質問のみ。説明は自分の言葉で書く）
> **最終更新：** 2026-10-23

生成AIに書いてもらったコードでも、自分で書いたコードでもかまいません。<br>
自分のアプリの中から3箇所を選んで、次の3つの問いに答えてください。<br>
分からないところは、生成AIに<br>
「この行は何をしていますか」「この行を消すとどうなりますか」<br>
と聞いてかまいません。<br>
ただし、書くのは自分の言葉です。<br>

---

## 箇所1: トラップ - 遅行

```swift
private func activateDoubleFlick(state: inout GameState) {
  state.doubleFlickTrapActive = true
  state.doubleFlickTrapMovesRemaining = max(1, Self.slowDurationMoves)
  state.slowForwardFlickProgress = 0
  let flicks = max(1, Self.slowForwardFlicksPerMove)
  let moves = max(1, Self.slowDurationMoves)
  let message = "トラップ! 前進には\(flicks)回フリックが必要。\(moves)マス進むと解除"
  state.message = message
  state.trapActivationMessage = message
    }
```

- **何をしているか：**
フリックを指定した回数入力しなければ1マス進めない効果を実装

- **なぜこう書くのか：**
他の罠と混在しないしないように関数でまとめた


- **もしこう書かなかったら：**
消しても特に異常なく操作できる

---

## 箇所2： トラップ - 不発

```swift
    private func unExploded(state: inout GameState) {
        let message = "トラップ！ しかしなにも起こらなかった"
        state.message = message
        state.trapActivationMessage = message
    }
```

- **何をしているか：**
トラップマスを踏んでも何もトラップが発生せず、メッセージが表示されるだけ

- **なぜこう書くのか：**
前述のトラップからアクションを差し引いた

- **もしこう書かなかったら：**
メッセージもなくしたら、バグなのかどうなのかわからなくなる

---

## 箇所3： トラップ - 罠の効果のパラメータ

```swift
struct TrapEvent: TileEvent {
    /// 毒が1歩の移動ごとに減らすHP。
    static let poisonDamagePerStep = 1
    /// 毒の持続歩数。罠マスへ入った移動も1歩として数える。
    static let poisonDurationSteps = 5
    /// 遅行中、1マス進むために必要な前進フリック数。
    static let slowForwardFlicksPerMove = 2
    /// 遅行が続く成功した前進移動の回数。
    static let slowDurationMoves = 5

    let kind: TrapKind
    let map: MapData

    init(kind: TrapKind, map: MapData) {
        self.kind = kind
        self.map = map
    }

```

- **何をしているか：**
それぞれの罠のダメージや歩数などの効果を発生

- **なぜこう書くのか：**
それぞれの罠のダメージや歩数などを手入力で修正できるようにしている

- **もしこう書かなかったら：**
メンテナンスが難しくなる

---

## 自分ではまだ説明できない箇所（正直に）

```swift
// 罠マスでのトラップを実行
struct TrapEvent: TileEvent {
    /// 毒が1歩の移動ごとに減らすHP。
    static let poisonDamagePerStep = 1
    /// 毒の持続歩数。罠マスへ入った移動も1歩として数える。
    static let poisonDurationSteps = 5
    /// 遅行中、1マス進むために必要な前進フリック数。
    static let slowForwardFlicksPerMove = 2
    /// 遅行が続く成功した前進移動の回数。
    static let slowDurationMoves = 5

    let kind: TrapKind
    let map: MapData

    init(kind: TrapKind, map: MapData) {
        self.kind = kind
        self.map = map
    }

    /// グループ振り分け
    func onEnter(state: inout GameState, from previousPosition: GridPos) {
        switch kind.group {
        case .a:
            activateGroupA(state: &state)
        case .b:
            activateGroupB(state: &state)
        case .c:
            activateGroupC(state: &state)
        }
    }

    // 進行妨害系
    private func activateGroupA(state: inout GameState) {
        switch kind {
        case .turnRight:
            turnPlayerRight(state: &state)
        case .doubleFlick:
            activateDoubleFlick(state: &state)
        case .hpDrain:
            activateHPDrain(state: &state)
        default:
            break
        }
    }

    /// 反転
    private func turnPlayerRight(state: inout GameState) {
        state.player.direction = state.player.direction.turnedRight
        let message = "トラップ！ 向きが右に90度変わった"
        state.message = message
        state.trapActivationMessage = message
    }

    /// 遅行
    private func activateDoubleFlick(state: inout GameState) {
        state.doubleFlickTrapActive = true
        state.doubleFlickTrapMovesRemaining = max(1, Self.slowDurationMoves)
        state.slowForwardFlickProgress = 0
        let flicks = max(1, Self.slowForwardFlicksPerMove)
        let moves = max(1, Self.slowDurationMoves)
        let message = "トラップ! 前進には\(flicks)回フリックが必要。\(moves)マス進むと解除"
        state.message = message
        state.trapActivationMessage = message
    }

    /// 毒
    private func activateHPDrain(state: inout GameState) {
        state.hpDrainTrapActive = true
        state.hpDrainTrapStepsRemaining = max(1, Self.poisonDurationSteps)
        let damage = max(0, Self.poisonDamagePerStep)
        let steps = max(1, Self.poisonDurationSteps)
        let message = "トラップ! 1歩ごとにHPが\(damage)減る。\(steps)歩で解除"
        state.message = message
        state.trapActivationMessage = message
    }

    // ワープ系
    private func activateGroupB(state: inout GameState) {
        switch kind {
        case .warpToStart:
            warpToStart(state: &state)
        case .randomWarp:
            randomWarp(state: &state)
        case .warpBeforeKey:
            warpBeforeKey(state: &state)
        default:
            break
        }
    }

    // 出戻りワープ
    private func warpToStart(state: inout GameState) {
        movePlayer(to: map.startPosition, state: &state)
        let message = "トラップ! スタート地点へ戻された"
        state.message = message
        state.trapActivationMessage = message
    }

    // ランダムワープ
    private func randomWarp(state: inout GameState) {
        let ordinaryFloorPositions = map.cells.indices.flatMap { y in
            map.cells[y].indices.compactMap { x in
                map.cells[y][x] == .floor ? GridPos(x: x, y: y) : nil
            }
        }
        // 行き先なし
        let destinations = ordinaryFloorPositions.filter { $0 != state.player.position }
        guard let destination = destinations.randomElement() else {
            let message = "トラップ! 行き先が見つからなかった"
            state.message = message
            state.trapActivationMessage = message
            return
        }
        // 行き先あり
        movePlayer(to: destination, state: &state)
        let message = "トラップ! 迷宮の別の場所へ飛ばされた"
        state.message = message
        state.trapActivationMessage = message
    }

    // 鍵前ワープ
    private func warpBeforeKey(state: inout GameState) {
        // マップ内の鍵の座標を探す機能
        func findKeyPosition() -> GridPos? {
            for y in map.cells.indices {
                for x in map.cells[y].indices where map.cells[y][x] == .key {
                    // 鍵の方向を向く
                    return GridPos(x: x, y: y)
                }
            }
            return nil
        }

        // 移動可能な床マスと鍵を向く方向
        func keyFrontPositions(for key: GridPos) -> [(position: GridPos, facingKey: Direction)] {
            let candidates: [(position: GridPos, facingKey: Direction)] = [
                (GridPos(x: key.x, y: key.y - 1), .south),
                (GridPos(x: key.x + 1, y: key.y), .west),
                (GridPos(x: key.x, y: key.y + 1), .north),
                (GridPos(x: key.x - 1, y: key.y), .east),
            ]
            return candidates.filter { map.cell(at: $0.position) == .floor }
        }

        // 鍵前ワープの候補から現在地に近いマスを選択
        func distance(_ first: GridPos, from second: GridPos) -> Int {
            abs(first.x - second.x) + abs(first.y - second.y)
        }

        // 鍵の位置を取得
        guard let keyPosition = findKeyPosition() else {
            failWarpBeforeKey(state: &state)
            return
        }

        // 鍵に隣接する床マスの発見
        let floorCandidates = keyFrontPositions(for: keyPosition)

        guard let destination = floorCandidates.min(by: {
            distance($0.position, from: state.player.position)
                < distance($1.position, from: state.player.position)
        }) else {
            failWarpBeforeKey(state: &state)
            return
        }

        completeWarpBeforeKey(to: destination, state: &state)
    }

    private func completeWarpBeforeKey(
        to destination: (position: GridPos, facingKey: Direction),
        state: inout GameState
    ) {
        movePlayer(to: destination.position, state: &state)
        state.player.direction = destination.facingKey
        let message = "ワープの罠！ 鍵の前へ飛ばされた"
        state.message = message
        state.trapActivationMessage = message
    }

    // 鍵か候補マスが見つからない場合は、失敗メッセージを設定して処理を終了
    private func failWarpBeforeKey(state: inout GameState) {
        let message = "ワープの罠！ 鍵の前へ移動できなかった"
        state.message = message
        state.trapActivationMessage = message
    }

    /// ワープ処理
    private func movePlayer(to destination: GridPos, state: inout GameState) {
        // ワープ先
        state.player.position = destination
        // 探索済み
        state.explored.insert(destination)
    }

    // グループC：運命系
    private func activateGroupC(state: inout GameState) {
        switch kind {
        case .forceGameOver:
            forceGameOver(state: &state)
        case .unexploded:
            unExploded(state: &state)
        default:
            break
        }
    }

    // 不発
    private func unExploded(state: inout GameState) {
        let message = "トラップ！ しかしなにも起こらなかった"
        state.message = message
        state.trapActivationMessage = message
    }
    
    // 強制ゲームオーバー
    private func forceGameOver(state: inout GameState) {
        state.playerHP = 0 //HP0に
        state.isCleared = false
        state.isGameOver = true
        state.enemyBattle = nil
        let message = "トラップ！ 強制的にゲームオーバーになった"
        state.message = message
        state.trapActivationMessage = message
    }

}
```
- **「動いているけれど何をしているか分からない」部部**
なぜマップの入り口、ゴール、鍵の配置を、把握できるのか。
