# 本仓库已不再更新，已经整合到：https://github.com/UnrealMultiple/TShockPlugin，后续更新均会发布至此仓库
# This repository is no longer updated and has been integrated into: https://github.com/UnrealMultiple/TShockPlugin. All future updates will be published there.
# PerPlayerLoot 插件

## 简介

**PerPlayerLoot** 是一个针对 Terraria 多人游戏 TShock 服务器的插件，旨在解决自然生成战利品箱资源分配不均的问题。该插件赋予每个战利品箱一个独立于其他玩家的库存，确保每位玩家在打开箱子时都能获得专属的战利品，即便其他人在之前已开过同一箱子。

### 问题背景

在 Terraria 的多人游戏中，由于世界空间及资源有限，率先探索地表或地下洞穴的玩家往往能垄断大量早期游戏战利品，尤其在非专家或大师模式下，这种现象尤为显著。这可能削弱后续玩家的游戏体验，特别是对于大型服务器而言，容易导致资源获取的失衡与探索动力的下降。

### 解决方案

PerPlayerLoot 插件通过模拟战利品箱内容，确保每个玩家在打开箱子时看到的是自己的个性化物品清单，从而保持探索与挖掘活动对所有玩家持续具有吸引力。

## 安装步骤

1. 将 **PerPlayerLoot.dll** 文件复制到 TShock 服务器的 **ServerPlugins** 目录中。
2. 重新启动服务器，创建一个**全新的世界**（非常重要，原因见下文）。
3. 启动游戏，尽情享受每个玩家专属的战利品箱吧！

## 工作原理

为了尽量减少对原版游戏逻辑的干扰以及对世界保存数据的影响，插件采取了以下策略：

- **数据包拦截**：插件截取与箱子交互相关的数据包，如 ChestPlace、ChestOpen 和 ChestItem。
- **伪造内容**：当玩家打开箱子时，插件从内部数据库发送定制的 ChestItem 数据包，呈现给玩家的是其个人化的战利品列表。
- **数据持久化**：每当世界保存时，玩家的个人战利品数据被写入 **perplayerloot.sqlite** 文件，同时记录所有玩家放置箱子的 X、Y 坐标。非排除列表中的玩家放置箱子被视为世界生成的战利品箱。

## 注意事项
- **插件安装时机**：为避免玩家放置的箱子被误识别，应在服务器开启之初即安装本插件。若在游戏过程中安装，所有现有箱子将被视为生成的战利品箱，其库存将为每位玩家复制。
- **罕见情况下的 Main.chest 修改**：仅在特定可能导致箱子物品丢失的罕见场景下，插件才会直接清空 Main.chest 数组中对应箱子的物品。
- **箱子数据库**：在tshock文件夹目录中的`perplayerloot.sqlite`

## 调试命令

- `/ppltoggle`：全局切换插件的数据包挂钩功能。**警告**：使用此命令可能导致 Main.chest 数组与插件内部状态不同步，不推荐在正常游戏环境中使用。启用此命令后，玩家放置的箱子将成为战利品箱，显示真实库存而非个人化库存。仅限调试目的！

通过引入 PerPlayerLoot 插件，TShock 服务器能够更好地支持大型多人游戏，确保每个玩家在有限的世界资源中都能享有公平且持续的探索乐趣。
