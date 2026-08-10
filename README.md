# 样本外 · 公开存档

这里存放每一期赛前推演和赛后比对的**原件**：图片，以及各平台发布用的成稿。

与八个发布平台上的内容一致。建这个仓库只有一个目的 —— 平台上的帖子可能
因为封号、举报或规则调整而消失，这里留一份不依赖任何平台的副本。
它是存档，不是第九个发布渠道，不在这里回复、讨论或提供任何额外内容。

## 目录

```
YYYY-MM-DD/forecast/          赛前推演：图 + 成稿（开赛前发布）
YYYY-MM-DD/settle/            赛后比对：图 + 成稿（赛后原样结算）
YYYY-MM-DD/SHA256SUMS         该期全部文件的哈希
YYYY-MM-DD/SHA256SUMS.ots     该期的 OpenTimestamps 时间戳
MANIFEST.csv                  全部文件的 SHA-256、字节数、出稿时刻
```

目录名是**销售日**。一个销售窗口跨两个日历日是常态（晚上到次日凌晨），
一个窗口算一期，不按日历切。

## 校验

单个文件：

```
certutil -hashfile <文件> SHA256      # Windows
shasum -a 256 <文件>                  # macOS / Linux
```

对照 `MANIFEST.csv` 的 `sha256` 列。

全量核对：

```bash
# macOS / Linux
tail -n +2 MANIFEST.csv | awk -F, '{print $5"  "$4}' | sha256sum -c -
```

```powershell
# Windows PowerShell
Import-Csv MANIFEST.csv | ForEach-Object {
  $h = (Get-FileHash $_.file -Algorithm SHA256).Hash
  if ($h -ne $_.sha256.ToUpper()) { "不一致: " + $_.file }
}
```

**哈希能证明什么：** 文件此后没有被改过。
**哈希不能证明什么：** 文件是哪天产生的。哈希是本仓库自己算的。

## 时间戳

每期目录下的 `SHA256SUMS.ots` 是 [OpenTimestamps](https://opentimestamps.org)
时间戳，把该期的哈希清单锚进了比特币区块链。

验证不需要信任本仓库，也不需要本仓库还活着：把同一目录下的 `SHA256SUMS`
和 `SHA256SUMS.ots` 一起拖到 https://opentimestamps.org 的验证框，
或者装 `ots` 客户端跑 `ots verify SHA256SUMS.ots`。

它证明的是：**这份哈希清单在某个比特币区块之前就已经存在。**
它不证明是谁做的，也不证明当时公开发表过 —— 后者由各平台帖子自己的
时间戳承担。两样拼起来才完整。

第 001 期（2026-08-09）的章是次日补盖的，所以那一期只能证明「不晚于
2026-08-10」。此后每期都在当日盖。

## 关于内容

- 全部为统计模型对公开历史数据的拟合推演，仅作数据挖掘与算法逻辑交流。
- 不提供任何操作建议，不构成任何形式的诱导。
- 「样本外」＝ out-of-sample。这里只收开赛前已公开、赛后原样比对的部分；
  历史回测属于样本内拟合，不作为业绩展示。
- 已发布的内容不删不改。修正以新增说明的方式追加。
