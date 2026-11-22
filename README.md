<p align="center">
  <a href="https://github.com/maple-days"><img src="https://github.com/maple-days.png?size=96" width="56" alt="Maple Days"></a>
</p>

<p align="center">
  <sub><b>Maple Days</b> · 《打脸日常》整合包 mod · Minecraft 1.12.2</sub>
</p>

<div align="center">
  <h2>Slashblade-Chixiao</h2>
  <p>拔刀剑附属：唐刀「赤零」与「赤霄」</p>
  <img width="256" height="256" alt="icon" src="https://github.com/user-attachments/assets/338f70fd-420b-43e8-9f26-4237e7c9b34f" />
</div>

---

## 这是什么

把《明日方舟·终末地》里**陈千语**的佩刀「赤霄」做成 1.12.2 的拔刀剑，并为它补上设定中的前身——「赤霄剑·第零式」，也就是**赤零**。

两把刀**共用同一套几何模型**，只有贴图不同，对应设定里"同一把剑的不同形态"。

| | 唐刀「赤零」 | 唐刀「赤霄」 |
|---|---|---|
| 定位 | 赤霄剑·第零式 | 老祖的剑 |
| 特殊攻击 | **冽风霜** | **绝影** |
| 基础攻击修正 | 4.0 + 钻石 | **5.0 + 钻石** |
| 最大耐久 | 72 | **90** |
| 附魔 | 耐久 III / 锋利 V / 抢夺 II | 耐久 III / 锋利 **VI** / 抢夺 **III** / 火焰附加 **II** |
| 获取 | 合成（见下） | 由赤零觉醒 |

## 特殊攻击

两把刀的 SA 都会**消耗 15 点耀魂**；耀魂不足时改为损耗耐久 10 点。

### 冽风霜（赤零）

沿视线依次放出 **7 道剑气**，蓝红交替，伤害逐波递增：

```
6 → 8 → 10 → 13 → 16 → 18 → 20      合计 91
```

### 绝影（赤霄）

连射 **7 把幻影剑**，前 6 把红刃、最后一击蓝刃：

```
10 × 6 + 30 = 90
```

两条 SA 的总伤刻意压在**同一量级**，区别在手感与视觉，而不是数值高低。

## 获取

### 赤零

```
耀魂宝珠  钻石块  耀魂宝珠
耀魂铁锭   铁剑   耀魂铁锭
耀魂宝珠   浮冰   耀魂宝珠
```

需要 **4 颗耀魂宝珠 + 2 块耀魂铁锭 + 1 个钻石块 + 1 把铁剑 + 1 块浮冰**。

### 赤霄（觉醒）

把**击杀数 ≥ 2000、耀魂 ≥ 5000** 的赤零放上工作台，搭配：

```
耀魂宝珠  红石块  耀魂宝珠
耀魂铁锭  赤零    耀魂铁锭
烈焰棒   钻石块   烈焰棒
```

赤零的 tooltip 会实时显示进度：

```
觉醒：击杀 1234/2000，耀魂 800/5000
```

达标时整行变绿。这两个门槛是「长期目标」而不是「顺手解锁」——考虑到 SA 每次还要吃 15 耀魂，实际进度会比单纯刷击杀更慢。

## 配置

`config/chixiao.cfg`

```ini
[balance]
    # 冽风霜伤害倍率（基础总伤 91）
    D:liefengshuangDamageScale=1.0
    # 绝影伤害倍率（基础总伤 90）
    D:jueyingDamageScale=1.0
```

实际伤害 = 基础上限 × 倍率。想要更温和填 `0.7`（冽风霜约 64），想要爽快填 `2.0`。改完**重启生效**，不需要重新编译。

## 兼容性

| 项 | 值 |
|---|---|
| Minecraft | 1.12.2 |
| 加载器 | Forge 14.23.5.2847+ / Cleanroom |
| 前置 | [SlashBlade](https://www.curseforge.com/minecraft/mc-mods/slashblade)（`required-after:flammepfeil.slashblade`） |
| 环境 | 客户端 + 服务端（双方都需要装） |
| 编译目标 | Java 8 |

## 构建

```bash
./gradlew build
# 产物：build/libs/Slashblade-Chixiao-<version>.jar
```

Gradle 需要 JDK 17+ 运行，编译目标为 Java 8。

## 实现说明

**SA 的 id 是全局共享的整数。** SlashBlade 的注册表是 `Map<Integer, SpecialAttackBase>`，
其他附属若占用同一个数字，就会互相覆盖——早期版本用 10/11，结果被别的附属抢走，
赤零挥出来的成了别人的 SA。

现在：

- id 固定在 `0x4D46`（"MF"）起的区段，冲突时顺延并打 `ERROR` 日志，不再静默失败
- 旧存档里的刀（NBT 存的是 10/11）会在玩家登录时**自动迁移**到新 id
- 版本号在 `gradle.properties` 与 `Tags.java` 各有一份，升版需同时修改

## 致谢

- **flammpfeil** —— SlashBlade 本体
- **鹰角网络 / 明日方舟·终末地** —— 赤霄的原始设计与陈千语的角色设定
- **MFn233** —— 两把刀的数值设计、SA 实现与模型

## 许可

代码以 **MIT** 发布；模型与贴图为本项目自制，第三方素材的版权归各自原作者。