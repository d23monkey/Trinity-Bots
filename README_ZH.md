### 更新时间表: 每周六上午5点 UTC+0

### 有问题吗？发现了一个错误？ [Issues](https://github.com/trickerer/Trinity-Bots/issues)

# [ NPCBOTS 操作手册 ]
>Compiled by: Trickerer (onlysuffering @ Gmail dot Com)  
>Version 0.25 - 14 Jun 2023
>Original version by: Thesawolf (@ Gmail dot Com) Version 0.3 - 20 July 2016 [here](https://github.com/thesawolf/TrinityCore/blob/TrinityCoreLegacy/README_Bots.md)

---------------------------------------
### 目录
1. [简介](#introduction)
2. [NPCBots](#npcbots)
    - [NPCBot Mod 安装指南](#npcbot-mod-installation)
    - [NPCBot 命令](#npcbot-commands)
    - [NPCBot 控制和用法](#npcbot-control-and-usage)
        - [NPCBot 入门](#npcbot-getting-started)
        - [NPCBot 机器人招募(可选)](#npcbot-hiring-alternatives)
        - [NPCBot 出行贴士](#npcbot-getting-around)
        - [NPCBot 装备](#npcbot-equipment)
        - [NPCBot 职责](#npcbot-roles)
        - [NPCBot 队形](#npcbot-formation)
        - [NPCBot 技能](#npcbot-abilities)
        - [NPCBot 天赋](#npcbot-talents)
        - [NPCBot 组队](#npcbot-grouping)
        - [NPCBot 附录](#npcbot-extras)
        - [NPCBots 坐骑](#npcbots-and-vehicles)
    - [NPCBot 漫游系统](#npcbot-wander-system)
    - [NPCBot Config 配置](#npcbot-config-settings)
    - [NPCBot Mod 本地化](#npcbot-mod-localization)
    - [NPCBot 附加职业](#npcbot-extra-classes)
    - [NPCBot 占位(待补充)](#npcbot-occupations)
    - [NPCBot 插件](#npcbot-addons)
3. [更新日志](#guide-changelog)

---------------------------------------
## 简介
本手册是为了阐述NPCBot系统的目的并解释其使用方法.


---------------------------------------
## NPCBOTS
NPCBots玩家视频教程可在[YouTube](https://www.youtube.com/watch?v=fByzoyl3rCY)上找到,由**QT Blue-AI**提供.

NPCBots是可雇佣的类似宠物的随从(有一些例外).你不能完全控制它们,但可以在很多方面调整它们的行为.机器人会跟随你,给你增益,保护你,并在总体上帮助你.它们的主要目的是在玩家升级期间提供支持,尽管它们可以进行地下城和队伍副本活动,但在这些活动中不要期望它们表现得很聪明.

NPCBots的特点：

- 战斗(通过法术、近战、远程战斗)

- 增益

- 治疗

- 复活

- 充当守卫(当机器人没有主人时)

- 组队

- 地下城查找器(它们可以和你一起进入地下城)

- 队伍副本(它们可以参加队伍副本)

- 玩家对战(它们可以与你的对立阵营成员战斗)

- 为你提供消耗品(法师、术士)

- 从你那里获得增益和消耗品

- 装备装备

- 分配角色

- 技能管理

- 等等...


### NPCBot Mod 安装指南
NPCBots 是一个基于 TrinityCore/AzerothCore 的模组(https://github.com/TrinityCore/TrinityCore/, https://github.com/azerothcore/azerothcore-wotlk/),仅支持 3.3.5 分支.

**TC 和 AC 的预打补丁仓库可在 [这里](https://github.com/trickerer/TrinityCore-3.3.5-with-NPCBots/) 和 [这里](https://github.com/trickerer/AzerothCore-wotlk-with-NPCBots/) 获取**  
如果你仍然偏好补丁,请继续阅读,否则克隆已打补丁的仓库并跳至步骤 4.

在本文档的最开始,你可以找到一个链接,指向 NPCBots 最后版本的 TrinityCore 修订版.如果你使用的是其他版本的 TrinityCore,无法保证你能够应用这个模组.

1. 遵循 TrinityCore 安装指南(https://TrinityCore.info/)首先安装服务器.
2. 下载 NPCBots.patch 并将其放入你的 TrinityCore 文件夹中.
3. 使用 `patch -p1 < NPCBots.patch` 命令应用补丁(创建新文件).
4. (重新)运行 CMake 并(重新)构建.
5. 将 worldserver.conf.dist 合并到你的 worldserver.conf 文件中(NPCBot 模组设置).
6. 将 `/TrinityCore/sql/Bots/` 中的 SQL 文件应用到你的数据库中(以 `characters_` 和 `world_` 开头的文件分别放入你的 `characters` 和 `world` 数据库)：
```
1_world_bot_appearance.sql
2_world_bot_extras.sql
3_world_bots.sql
4_world_generate_bot_equips.sql
5_world_botgiver.sql
characters_bots.sql
```
7. 将 `/TrinityCore/sql/Bots/updates` 中的 SQL 更新文件应用到你的数据库中.

```
提示：对于新安装,还有 shell 脚本可供你快速合并所有必需的 SQL 文件到 `ALL_auth.sql`, `ALL_chracters.sql` 和 `ALL_world.sql`,分别进入 `auth`, `characters` 和 `world` 数据库.
```

之后,你就可以开始使用了.
### NPCBot 命令
首先,要列出你的NPCBot的属性,使用 `/bonk` 命令(警告：列表很长)

请注意,某些命令可能并非所有账户都可用(取决于他们在RBAC表(TC)中的访问级别和权限设置 / 在 `command` 表(AC)中).你可能需要更改你的账户权限/安全级别以启用某些命令的使用
大多数NPCBot命令根据权限分为两组：玩家命令和GM命令,还有一些仅限管理员使用的命令
```
KEY:
< >  (尖括号)表示必需的命令参数  
[ ]  (方括号)表示可选的命令参数  
 |  (管道符号)表示参数选择(即 this|that  = this 或者 that)  
_ARGUMENT_ 表示参数名称  
```
**命令**：**`.npcbot`**, **`.npcb`** -- (玩家命令)单独使用将列出所有可用的语法

- **`lookup <_CLASS_> [_UNSPAWNED_ONLY_]`** -- (GM命令)通过 <CLASS> 查找NPCBot条目,返回NPCBots的ID、名称和种族列表  
    - _CLASS_ = 类ID(例如1代表战士).**`.npcbot lookup` (`.npcb loo`)** (不带参数)列出类ID  
    - _UNSPAWNED_ONLY_ = 布尔标志,如果设置为1,只显示尚未生成的NPCBots  
    **示例用法**：  
        - `.npcbot lookup 7` (列出所有萨满)  
        - `.npcb loo 11 1` (列出所有未生成的德鲁伊)  
- **`list`** -- (GM命令)  
    - **`spawned`** -- 列出世界上所有生成的机器人,它们的位置和快速状态  
        - **`free`** -- 同上,但只列出**无主**的机器人  
            **示例用法**：  
                - `.npcbot list spawned`  
                - `.npcbot list spawned free`  
                - `.npcbot list s f`  
- **`add _TARGET_`** -- (GM命令)选择的NPCBot成为你的,绕过价格条件.只有在NPCBots没有所有者时才有效  
    - _TARGET_ = 选中的NPCBot  
    **示例用法**：  
        - `.npcbot add`  
        - `.npcb add`  
- **`remove _TARGET_`** -- (GM命令)从玩家控制中解散NPCBot(s).通过此命令移除的任何机器人都会保留其装备  
    - _TARGET_ = 选中的NPCBot(解散选中的NPCBot)  
    - _TARGET_ = 选中的玩家(解散所有NPCBots)  
    **示例用法**：  
        - `.npcbot remove`  
        - `.npcb rem`  
- **`spawn <_ENTRY_|_LINK_>`** -- (GM命令)在世界中生成NPCBot,NPCBot保存在数据库中.只有在世界地图中有效(不在实例中).请注意,与其他生物不同,每种NPCBot只能生成一个实例,但别担心：有很多NPCBots可供选择  
    - _ENTRY_ = NPCBot的ID(可以从查找列表中获得)  
    - _LINK_ = Shift-click添加的生物模板链接(从查找列表中获得)  
    **示例用法**：  
        - `.npcbot spawn 70001` (生成ID为70001的NPCBot)  
        - `.npcb sp 70002` (生成ID为70002的NPCBot)  
        - `.npcb sp [哈洛姆]` (通过链接生成NPCBot)  
- **`move <_ENTRY_|_LINK_|_TARGET_>`** -- (GM命令)将生成的NPCBot移动到新位置.此命令替换了机器人的`.npc move`命令  
    - _ENTRY_ = NPCBot的ID  
    - _LINK_ = Shift-click添加的生物模板链接(从查找列表中获得)  
    - _TARGET_ = 选中的NPCBot  
    **示例用法**：  
        - `.npcbot move 70001` (将ID为70001的NPCBot移动到你的位置)  
- **`delete`** -- (GM命令)从世界中删除NPCBot,如果有任何所有者,则从其所有者中移除NPCBot并从数据库中删除.**装备将退还给机器人的所有者(如果有的话)或使用命令的玩家**.如果你需要让机器人的所有者**不**收回装备,在删除之前使用`.npcbot remove`移除机器人.如果在控制台对**无主但装备**的NPCBots使用,命令将失败  
    - _TARGET_ -- 删除选中的NPCBot  
        - _TARGET_ = 选中的NPCBot  
        **示例用法**：  
            - `.npcbot delete`  
            - `.npcb del`  
    - id _ID_ -- 使用生物ID而不是目标来删除NPCBots,可以从控制台使用  
        - _ID_ = 生物ID(`creature_template`.`entry`)  
        **示例用法**：  
            - `.npcbot delete id 70032`  
            - `.npcb del id 70032`  
    - free -- 删除**所有**无主的NPCBots,可以从控制台使用  
        **示例用法**：  
            - `.npcbot delete free`  
            - `.npcb del f`  
- **`set`** (GM命令)  
    - **`faction <a|h|m|f|_factionID_> _TARGET_`** -- 为选中的NPCBot设置派系  
        - a = 1802(联盟队伍)  
        - h = 1801(部落队伍)  
        - m = 14(怪物,对所有人敌对)  
        - f = 35(对所有人友好)  
        - _factionID_ = FactionTemplate.dbc中的ID(仅限专家).这不是你通过`.lookup faction`命令获得的  
        - _TARGET_ = 选中的NPCBot  
        **示例用法：**  
            - `.npcbot set faction` (单独使用将显示派系的子命令列表)  
            - `.npcb s f m` (将选中的NPCBot的派系设置为对所有人敌对)  
    - **`owner <_GUID_|_NAME_> _TARGET_`** -- 将选中的NPCBot的所有权设置为特定玩家  
        - _GUID_ = 玩家数据库全局唯一标识符  
        - _NAME_ = 玩家名称  
        - _TARGET_ = 选中的NPCBot  
        **示例用法：**  
            - `.npcbot set owner 312` (将选中的NPCBot的所有者设置为GUID为312的玩家)  
            - `.npcb s o Myplayer` (将选中的NPCBot的所有者设置为名为`Myplayer`的玩家)  
    - **`spec <_NUMBER_> _TARGET_`** -- 强制为选中的NPCBot更改专精  
        - _NUMBER_ = 从**1**到**30**的数字  
        - _TARGET_ = 选中的NPCBot  
        **示例用法：**  
            - `.npcbot set spec 2` (选中的NPCBot将立即重新专精为狂怒天赋树；天赋只适用于战士NPCBot)  
- **`revive _TARGET_`** -- (GM命令)复活NPCBot(s)  
    - _TARGET_ = 选中的NPCBot(复活这个NPCBot)  
    - _TARGET_ = 选中的玩家(复活选中玩家的所有NPCBots)  
    **示例用法：**  
        - `.npcbot revive`  
        - `.npcb rev`  
- **`reloadconfig`** -- (GM命令)重新加载NPCBot系统设置  
    - (无参数)  
    **示例用法：**  
        - `.npcbot reloadconfig`  
- **`command`** -- (玩家命令)允许你管理你的NPCBots定位、移动和其他一些事情(单独使用将显示子命令列表)  
    - **`follow _TARGET_`** -- 将NPCBot(s)设置为跟随模式  
        - **`only`** -- 切换NPCBots的非活动模式.非活动机器人将不会做任何事情,只会跟随  
    - **`standstill _TARGET_`** -- 将NPCBot(s)设置为停留模式  
    - **`stopfully _TARGET_`** -- 将NPCBot(s)设置为完全停止模式  
        - _TARGET_ = 选中的(你的)NPCBot(命令影响这个NPCBot)  
        - _TARGET_ = 任何其他单位或无选择(命令影响你所有的NPCBots)  
    - **`nocast`** -- 切换NPCBots施放任何法术的技能  
    - **`nolongcast`** -- 切换NPCBots施放非零施法时间法术的技能  
    - **`unbind <_TARGET_|_NAMES..._>`** -- 临时释放NPCBot(s)而不解散它们.机器人(s)将返回到起始位置并在那里等待,直到被邀请回来(或服务器重启)  
    - **`rebind <_TARGET_|_NAMES..._>`** -- 将未绑定的NPCBot(s)召回.使用 `.npcbot info` 列出你的未绑定NPCBots
    - _目标_ = 选中的(你的)NPCBot(命令影响此NPCBot)
    - _名字..._ = 不区分大小写的NPCBot名字,*包含空格的名字必须用下划线代替*(命令影响指定名字的NPCBot(s))
    - **`walk`** -- 切换NPCBots的行走模式
    - **`nogossip`** -- 切换NPCBots的对话可用性
    **示例用法：**
        - `.npcbot command standstill`(NPCBot停止移动并将保持位置)
        - `.npcb co sta`(同上)
        - `.npcbot command stopfully`(NPCBot将中断所有动作,停止并不对任何事物做出反应)
        - `.npcbot command follow`(如果NPCBot尚未跟随,则NPCBot将跟随你)
        - `.npcbot command follow only`(所有NPCBots在跟随你时将不做任何事情)
        - `.npcb co nog`(即使你不在战斗中,也将无法打开NPCBot对话菜单)
        - `.npcb co unb sheal`(暂时移除德鲁伊NPCBot Sheal)
- **`info _TARGET_`** -- (玩家命令)显示拥有的机器人信息
    - _目标_ = 选中的组队玩家或自己(显示该玩家的信息)
    **示例用法：**
        - `.npcbot info`
        - `.npcb in`
- **`hide`** -- (玩家命令)强制NPCBots暂时消失.它们将被传送出地图和世界,直到被允许回来.在战斗中不能使用
    - (无参数)
    **示例用法：**
        - `.npcbot hide`
        - `.npcb h`
- **`unhide`|`show`** -- (玩家命令)与 `.npcbot hide` 命令相反；你的NPCBots将很快出现.在战斗中不能使用
    - (无参数)
    **示例用法：**
        - `.npcbot unhide`
        - `.npcbot show`
- **`sendto <_TARGET_|_NAMES..._>`** -- (玩家命令)在30秒内使NPCBot(s)等待你的信号移动到固定位置.然后必须通过目标法术(AoE、烟雾信号弹、炸药等)指向该位置.定位的机器人将无限期地留在那里,使用跟随命令将它们召回.处于FULLSTOP状态的机器人不能被此移动
    - **`last <_TARGET_|_NAMES..._>`** -- (玩家命令)与简单的 `sendto` 相同,但将NPCBot(s)移回机器人发送的**起始**位置,使其固定
    - **`point <NUMBER> <_TARGET_|_NAMES..._>`** -- (玩家命令)将NPCBot(s)移动到之前标记的站点点
        - **`set <_NUMBER_> <_TARGET_|_NAMES..._>`** -- (玩家命令)通过 `<_数字_>` 标记NPCBots的当前位置为站点点
    - _数字_ = 范围在 `1 ... 5` 内的整数
    - _目标_ = 选中的NPCBot(移动单个NPCBot)
    - _名字..._ = 以空格分隔的不区分大小写的NPCBot名字,*包含空格的名字必须用下划线代替*(移动一组NPCBots)
    **示例用法：**
        - `.npcbot sendto`
        - `.npcb send eva jol eanor harene`
        - `.npcbot sendto last eva jol eanor harene`
- **`recall _TARGET_`** -- (玩家命令)强制NPCBot直接移动到你的位置.即使死亡时也可使用.主要用于当你死亡时,你的NPCBots卡在纹理下并且同时处于战斗中的情况
    - **`teleport _TARGET_`** -- (玩家命令)强制NPCBots立即*传送*到你的位置.在PvP中不能使用
    - **`spawns`** -- (玩家命令)强制你的非活动NPCBots立即传送到它们的生成位置.如果登出后机器人没有重新加入你,请使用
    - _目标_ = 选中的NPCBot(移动单个NPCBot)
    - _目标_ = 自己(移动所有NPCBots)
    **示例用法：**
        - `.npcbot recall`
        - `.npcb rec tele`
- **`kill _TARGET_`|`suicide _TARGET_`** -- (玩家命令)强制NPCBot死亡.主要用于当NPCBots不正常行动时的故障排除.这可能是由一个罕见的错误引起的,导致生物保留单位状态.如果这不起作用,尝试 `/tickle` 它们
    - _目标_ = 选中的NPCBot(杀死单个NPCBot麻烦制造者)
    - _目标_ = 自己(杀死所有你的NPCBots)
    **示例用法：**
        - `.npcbot kill`
        - `.npcbot suicide`
- **`order`** -- (玩家命令)允许你向你的NPCBot发出命令.命令优先于任何其他动作.每个机器人一次最多可以有3个排队的命令(它本身将显示子命令列表)
    - **`cast <_BOT_NAME_ OR _CLASS_NAME_> <_SPELL_NAME_> [_TARGET_TOKEN_]`** -- 施放一些法术
        - _BOT_NAME_ 或 _CLASS_NAME_ = 客户端中你的机器人名字,不区分大小写 OR 英文中的机器人类别名字,小写,*包含空格的名字必须用下划线代替*
        - _SPELL_NAME_ = 客户端中的法术名字.所有空格必须用下划线替换.不区分大小写
        - _TARGET_TOKEN_ = 可选的目标标识符字符串.如果留空,机器人将自我施放.不区分大小写.可能的值：
            - `bot`, `self` = 自我施放
            - `me`, `master` = 机器人所有者(你)
            - `target` = 机器人的当前目标(如果机器人没有目标,则不工作)
            - `mytarget` = 你当前的目标(如果你没有目标,则不工作)
    **示例用法：**
        - `.npcbot order cast javad lesser_healing_wave me`
        - `.npcbot order cast javad purge mytarget`
- **`useonbot`** -- (玩家命令)允许你使用你的技能,只针对NPCBots上的玩家.复活,像Intervene、Misdirection等队伍法术.这绕过了任何*客户端*限制,防止选择机器人作为这些法术的目标
    - **`spell <_SPELL_ID_ OR _SPELL_LINK_ OR _SPELL_NAME_>`** -- 使用技能
        - _SPELL_ID_ = 法术id
        - _SPELL_LINK_ = 从法术书中shift-click到聊天的结果
        - _SPELL_NAME_ = 从法术书中shift-click到宏窗口的结果,要么是 `法术名字` 或 `[法术名字]`.区分大小写
    - **`item <_ITEM_ID_ OR _ITEM_LINK_ OR _ITEM_NAME_>`** -- 使用物品
        - _ITEM_ID_ = 法术id
        - _ITEM_LINK_ = 从法术书中shift-click到聊天的结果
        - _ITEM_NAME_ = 从法术书中shift-click到宏窗口的结果,要么是 `物品名字` 或 `[物品名字]`.区分大小写
    **示例用法：**
        - `.npcbot useonbot spell [Resurrection]`
        - `.npcbot useonbot spell Dampen Magic`
        - `.npcbot useonbot spell Intervene`
        - `.npcbot useonbot item [Gnomish Army Knife]`
        - `.npcb use s Intervene`
- **`distance <_VALUE_>`** -- (玩家命令)允许你快速设置机器人跟随距离(单独使用将显示完整帮助)
    - **`attack <_VALUE_|_SHORT_|_LONG_>`** -- (玩家命令)设置攻击距离
        - _值_ = 所选距离类型的期望值(在标准距离范围内)
        - _短_, _长_ = “短”和“长”的确切字符串,用于该职业可用的最小和最大法术范围
    **示例用法：**
        - `.npcbot distance 75`
        - `.npcbot distance attack 20`
        - `.npcb dist a short`
- **`vehicle eject _TARGET_`** -- (玩家命令)允许你将你的NPCBots从载具中踢出(在战斗中机器人不会自动从载具中下来)
    - _目标_ = 选中的NPCBot(弹出此NPCBot)
    - _目标_ = 自己(弹出所有NPCBots)
    **示例用法：**
        - `.npcbot vehicle eject`
        - `.npcb veh e`
- **`go _ENTRY_`** -- (管理员命令)允许你传送到NPCBot当前的位置,类似于玩家的`.appear`命令.请注意,由于生物路径错误,这个命令有时会将你传送到地面以下.
    - _ENTRY_ = 生物ID  
    **示例用法：**  
        - `.npcbot go 70855` (传送到NPCBot 70855)  
        - `.npcb go 70855`  
- **`wp`** -- (管理员命令)一组用于操作自由漫游机器人的漫游点的开发命令.如果你想使用它们,请查看代码  
    - **`spawnall`**  
    - **`move`**  
    - **`add`**  
    - **`del`**  
    - **`list`**  
    - **`list all`**  
    - **`go`**  
    - **`setlevels`**  
    - **`setlevels z`**  
    - **`setflags`**  
    - **`setflags z`**  
    - **`setname`**  
    - **`setlinks`**  
    - **`info`**  
    - **`links`**  
- **`dump`** -- (管理员命令)允许你迁移机器人数据,类似于玩家的`pdump`(单独使用将显示子命令列表)  
    - **`write <_FILENAME_>`** -- 创建一个包含将机器人移动到另一个数据库所需的信息的备份文件  
        - _FILENAME_ = 要创建的文件名,将保存在服务器根文件夹(Windows)或主目录(Linux),如果没有提供文件扩展名,将使用**.sql**  
        **示例用法：**  
            - `.npcbot dump write bots_backup` (写入`bots_backup.sql`)  
            - `.npcb du w 1.txt` (写入`1.txt`)  
    - **`load <_FILENAME_> [_KICK_PLAYERS_]`** -- 从备份文件加载NPCBots信息到数据库.需要没有玩家在玩(使用控制台)并且完成后将强制服务器重启.NPCBots模组必须已经安装(所有表格都存在)  
        - _FILENAME_ = 你的备份文件名,必须存储在服务器根文件夹(Windows)或主目录(Linux),如果没有提供文件扩展名,将使用**.sql**  
        - _KICK_PLAYERS_ = 布尔标志,如果设置为1,所有玩家将自动从服务器踢出  
        **示例用法：**  
            - `.npcbot dump load bots_backup` (从`bots_backup.sql`加载)  
            - `.npcb du l 1.txt` (从`1.txt`加载)  
- **`createnew <_NAME_> <_CLASS_> _RACE_ _GENDER_ _SKIN_ _FACE_ _HEARSTYLE_ _HAIRCOLOR_ _FEATURES_ _SOUNDSET_`** -- (管理员命令)允许你为玩家创建新的NPCBots.为此使用70800+的生物ID.  
    - _NAME_ = 创建的NPCBot的名称.注意,首字母将始终为大写,*包含空格的名称必须用下划线表示*  
    - _CLASS_ = 这指的是NPCBot的类别.使用`.npcbot lookup`命令列出所有可用的类别  
    - _RACE_ = 除非你正在创建一个特殊类别的NPCBot,其种族以及其他细节已经预定义,否则你必须为你的新NPCBot提供一个种族  
        - 1: 人类  
        - 2: 兽人  
        - 3: 矮人  
        - 4: 暗夜精灵  
        - 5: 亡灵  
        - 6: 牛头人  
        - 7: 侏儒  
        - 8: 巨魔  
        - 10: 血精灵  
        - 11: 德莱尼  
    - _GENDER_ = 与_RACE_相同,对于普通类别,你必须选择一个  
        - 0: 男性  
        - 1: 女性  
    - _SKIN_, _FACE_, _HEARSTYLE_, _HAIRCOLOR_, _FEATURES_ = 外观细节,从`0`开始向上.不同的种族/性别组合对视觉有不同的限制,使用`.npcbot createnew ranges`查看它们  
    - _SOUNDSET_ = NPC声音变体供机器人使用.每种种族有3种变体,默认情况下选择是随机的  
    **示例用法：**  
        - `.npcbot createnew Selendris 8 10 1 6 4 3 2 4` (创建一个女性红发血精灵法师NPCBot)  
        - `.npcb cre Selendris 8 10 1 6 4 3 2 4` (创建一个女性红发血精灵法师NPCBot)  
        - `.npcb createnew ranges` (打印所有种族的视觉限制)

### NPCBot 控制和用法
#### NPCBot 入门
如果你是第一次使用NPCBot,你需要按照以下步骤开始：

- `.npcbot lookup`  

这将给你一个可用类别的列表,每个类别都有一个ID来表示.例如,1是战士的类别ID.

_示例输出_：
```
.npcbot lookup #class
根据#class查找npcbots,并返回所有匹配项及其生物ID.
BOT_CLASS_WARRIOR = 1
BOT_CLASS_PALADIN = 2
BOT_CLASS_HUNTER = 3
BOT_CLASS_ROGUE = 4
BOT_CLASS_PRIEST = 5
BOT_CLASS_DEATH_KNIGHT = 6
BOT_CLASS_SHAMAN = 7
BOT_CLASS_MAGE = 8
BOT_CLASS_WARLOCK = 9
BOT_CLASS_DRUID = 11
BOT_CLASS_BLADEMASTER = 12
BOT_CLASS_SPHYNX = 13
BOT_CLASS_ARCHMAGE = 14
BOT_CLASS_DREADLORD = 15
BOT_CLASS_SPELLBREAKER = 16
BOT_CLASS_DARK_RANGER = 17
BOT_CLASS_NECROMANCER = 18
BOT_CLASS_SEA_WITCH = 19
BOT_CLASS_CRYPT_LORD = 20
```

在你确定了你想要查找的类别之后,输入：

- `.npcbot lookup 1`
```
查找类别1的机器人...
70001 - [Llane] 人类
70002 - [Thran] 矮人
70003 - [Lyria] 人类
70004 - [Ander] 矮人
70005 - [Malosh] 兽人
70006 - [Granis] 矮人
...
70038 - [Kerra] 血精灵
```

接下来,你将从列表中选择一个NPCBot：

- `.npcbot spawn 70003`  
在这个例子中,我们选择ID为70003的Lyria

Lyria将在你的位置上作为一个80级的战士出现(默认情况下,NPCBots以最高等级出现)  
请注意你生成的NPCBot是友好的.默认情况下,NPCBots的阵营ID设置为35(对所有人友好),但NPCBots将遵循领域PvP规则,即使它是FFAPvP  
此外,NPCBots不会攻击任何东西,除非被挑衅,在这种情况下,他们可能会尝试尽一切努力杀死他们的目标；他们可以非常坚持  
**NPCBots只能在世界地图上生成**

右键点击NPCBot将打开一个_对话菜单_(这会给你一些命令选择)  
_示例输出_：
```
你需要什么？
- 我需要你的服务
- 没事了
```

注意：如果你启用了GM模式,你还会看到一个'\<调试\>'菜单

接下来,你很可能只会选择'\<雇佣机器人\>',这将弹出一个确认框,询问：  
“你愿意雇佣Lyria吗？”,有一个费用金额,你可以_接受_或_取消_.
注意：价格会随着你的等级而变化,但稀有和精英机器人的费用更高,可能需要你至少达到X级才能雇佣它们

NPCBot被雇佣后,它们将自动与你等级匹配.  
右键点击NPCBot将打开一个新的对话菜单,有各种选项(在后续小节中描述).你的NPCBot会跟随你,无论是在队伍内还是队伍外,但最好让他们加入你的队伍,这样你就可以在小地图上监控他们的位置,或者健康/法力等.你新雇佣的对话菜单将显示：
```
- 机器人 装备 管理...
- 机器人 职责 管理...
- 机器人 队形 管理...
- 机器人 技能 管理...
- 机器人 天赋 管理...
- 给予消耗品、合剂等...
- <创建队伍>
- 你被解雇了!
- 该死的,振作起来!
- 没事了
- [这里可能显示可选选项]
- <创建队伍(所有机器人)>
- <添加到队伍>
- <将所有机器人添加到队伍>
- <从队伍中移除>
- [这里可能显示特定类别的选项]
```

现在,选择'\<创建队伍\>',你的NPCBot将加入你的队伍,你可以开始你的冒险！  
如前所述,其他选项将在本文档的后面进一步讨论.

#### NPCBot 机器人招募(可选)
如果你愿意,你还可以生成一个提供NPCBots雇佣服务的NPC.这是通过`.npc add`命令以正常方式完成的：

- `.npc add 70000`  
右键点击NPC将打开一个_对话菜单_：
```
并不总是有人愿意为了钱去卖命.
- 我需要你的服务
- 没事了
```

在菜单下,你将找到一个NPCBots类别的列表.在你选择了一个之后,一个对你可用的世界中的NPCBots列表将会出现.  
请注意,你通过这种方式雇佣NPCBots需要满足的条件与你通常会有的条件完全相同,唯一的区别是你不需要在世界各地寻找你需要的NPCBots.

#### NPCBot 出行贴士
无论是否组队,你的NPCBot会始终伴你左右,一路上为你提供增益(如果他们能增益的话),在需要时为你治疗(如果它是治疗型的NPCBot),与你并肩作战,甚至在你死亡时复活你(如果他们能复活的话).NPCBots被设计成能够跟上你的跑步速度,并且在你骑乘时会自行骑上他们自己的版本坐骑.如果他们跟不上(因为你移动得太快或者他们被困在战斗中或其他原因),你的NPCBot最终会传送到你的位置(即使你进入另一个地图).
注意：如果你不在他们的小组中,NPCBots将不能在副本中传送到你那里.

在大多数情况下,你去某个地方,它们就会跟随.就这么简单.
在那些跟随可能不安全或者你想安全前进的情况下,你有几个选择.

如果你的NPCBot就在你的直接附近,你可以选择它们并做出表情：
- `/stand` 让你的NPCBot停下
- `/wave` 让你的NPCBot再次跟随你

如果你的NPCBot不能被选中或者不在即时附近(可供选择),你可以使用命令：

- `.npcbot command stay` (`.npcb c s`) 让你控制的所有NPCBots 停下

- `.npcbot command follow` (`.npcb c f`) 让你控制的所有NPCBots 跟随

如果你的NPCBot离你太远,无法找到路径到你这里,你的NPCBot会将自己传送到你这里.
你也可以使用 `.npcbot recall teleport` 命令强制你的NPCBots传送到你这里

当你离开这个世界时,你的NPCBots不会在你决定退出的地下城外闲逛.除非你在那个地下城外生成了那个NPCBot.当你登出时,你的NPCBots暂时变成普通的自由NPCBots(但由于它们已经有主人了——就是你,所以不能被雇佣),并返回它们的生成地点.如果你在达纳苏斯捡到了你的NPCBot,它会回到那里.如果你在穿越贫瘠之地的路上生成了你的NPCBot,它会回到那里.这可能既烦人又有好处,在一个好的地方(比如城市里)生成NPCBots,将为你提供一个容易雇佣它们的方式(顺便说一下,它们喜欢闲逛并给路人增益).

#### NPCBot 装备
NPCBots 出生时会携带一些基本装备和衣物.然而,一些 NPCBots 会携带一些相当强大的武器,但这些武器只是视觉效果,不提供任何好处：没有伤害,没有属性加成.什么都没有.它们是**纯粹视觉的**.实际上,在大多数情况下,这些武器会部分地根据职业外观而改变,并且会一直保留(比如黑暗游侠和她信赖的弓).
NPCBots 允许你定制他们的个人装备部件,使他们在战斗中更加有效.请注意,NPCBot 装备的视觉变化不是即时的(除了武器).

要更改他们的装备,你需要右键点击那个 NPCBot,并从他们雇用后的对话菜单中选择 `机器人 装备 管理...`.然后你应该看到以下内容：
```
- 让我看看你的装备
- 自动筛选可用装备...
- 主手武器...
- 远程武器...
- 头部...
- 肩膀...
- 胸部...
- 腰部...
- 腿部...
- 脚...
- 手腕...
- 手...
- 披风...
- 衬衣...
- 戒指1...
- 戒指2...
- 饰品1...
- 饰品2...
- 颈部...
- 卸下全部装备(退回到背包)
- 刷新机器人外观
- 返回
```
如你所见,你可以为 NPCBot 的几乎所有装备槽位装备装备.

- `让我看看你的装备` 会让 NPCBot 私聊给你他们当前拥有的所有东西的列表,包括一个物品图标(有助于理解物品装备在哪个槽位)

- `自动筛选可用装备` 会列出你背包中 NPCBot 可以使用的所有物品.点击这些物品中的任何一个,会自动将其交给 NPCBot 并装备到适当的槽位

- `_(个人装备槽位)_` 会显示他们装备了什么(如果有的话),使用他们旧装备的选项(如果有的话)或者卸下装备的选项(如果你给了他们一些装备),列出你背包中 NPCBot 可以在那个槽位使用的任何物品和一个返回选项.选择你的背包中的任何物品都会自动将该物品发送给 NPCBot 并让他们装备它.使用旧装备或卸下装备的选项会让 NPCBot 将你给他们的装备返回到你的背包中.然后他们会回到该槽位的默认装备/状态.如果启用了此功能,你还可以使用你库存/背包中的物品对显示的装备槽位进行幻化,参考世界服务器配置中的幻化规则

- `卸下全部装备(退回到背包)` 会让他们这样做...将你给他们的所有装备都倒回你的背包中.如果你的背包空间不够,多余的物品将通过邮件发送给你.注意：当解雇一个 NPCBot 时,你给他们的任何装备都会自动返回给你

- 此菜单中还有一个额外的可选项目称为 `机器人装备银行...`,可以通过在配置中设置 `NpcBot.GearBank.Enable = 1` 来启用.它提供了无限的装备存储空间 - 当你有很多机器人,无法将所有额外的装备都保留在自己的背包中时非常有用.这当然是一种利用,所以这个功能默认是禁用的.请注意,这个存储是每个玩家保存的,所以你不会在最后一个机器人被解雇时丢失这个装备,但同时,如果没有机器人可以交谈,你也无法访问它

- `返回` 只是返回到上一个菜单

#### NPCBot 职责
NPCBot 职责管理允许你调整他们的整体运作方式.可用的选项取决于你控制的 NPCBot 的职业

要调整 NPCBot 的职责,你需要右键点击那个 NPCBot,并从他们的对话菜单中选择 `管理职责...`.然后你应该看到(取决于职业)：
```
- 收集材料...
- 自动拾取...
- 坦克
- 副坦克
- 输出
- 治疗
- 远程
- 返回
```
点击相应的职责将切换它的状态(改变图标)

职责可能有点难以理解：

- "坦克" 职责意味着 NPCBots 将尽可能产生更多的威胁,使用嘲讽类技能重新吸引攻击朋友的敌人,并更自由地使用防御性冷却技能.这不包括攻击任何东西.没有启用 "副坦克" 的 "坦克" Bot 被视为主坦克,并且将始终停留在 `TankTargetIconMask`(见 [配置 设置](#npcbot-config-settings))指向的目标上

- "副坦克" 是 "坦克" 职责的补充,使坦克 NPCBots 优先考虑 `OffTankTargetIconMask` 指向的目标.启用此职责也会自动启用 "坦克" 职责

- "输出" 职责允许 NPCBots 实际造成伤害.如果此职责被禁用,NPCBot 将不会使用造成伤害的技能,甚至不会自动攻击

- "治疗" 是你的治疗者需要启用的职责.如果此职责被禁用,NPCBots 甚至不会尝试治疗任何东西,即使是他们自己.不,即使是面对必死无疑的情况

- "远程" 职责影响 NPCBots 的定位和他们将与攻击目标保持的距离.远程伤害输出非坦克 Bot 将始终攻击 `RangedDPSTargetIconMask` 指向的目标
_示例_：启用了坦克 + 伤害输出 + 远程职责的战士将不断尝试嘲讽目标并逃跑,只有在目标追上时才会攻击

建议只为该职业启用 1 或 2 个特定职责,以最小化他们频繁切换战术的情况.唯一的例外是牧师,他们可以很好地处理伤害输出、治疗和远程职责(他们会保留一些法力用于治疗,并转而使用魔杖)

`收集材料...` 职责允许 NPCBots 收集不同的矿石、草药、皮革和其他贸易商品.它不允许跟踪这些商品,所以祝你好运.它也不允许机器人制作任何东西.NPCBots 的技能根据他们的等级分配,所以例如 1 级的 NPCBot 将无法采矿.请注意,机器人只会使用 **你** 非常接近的节点

`自由拾取...` 职责允许 NPCBots 为你和你的队伍中的其他玩家自动且快速地收集附近可拾取生物的物品.确保为你的队伍选择拾取方法,质量阈值,并为你的拾取机器人设置拾取设置

#### NPCBot 队形
有时候你可能希望NPCBot离你近一些,或者尽可能远.队形选项允许你调整NPCBot与你的距离.

从他们的对话菜单中选择“机器人 队形 管理...”来调整队形.你将看到：
```
- 跟随距离(当前：XX)
- 禁用战斗定位
- 攻击距离...
- 攻击方向...
- 参与行为...
- 优先目标(坦克)...
- 优先目标(输出)...
- 返回
```
注1：如果NPCBot被分配了“远程”角色,你将看到“禁用战斗定位”、“攻击距离...”和“攻击方向...”选项.
注2：如果NPCBot不是坦克,你将看到“参与行为”选项.
注3：如果NPCBot具有所需的角色,你处于队伍中,并且至少有一个属于相应目标图标掩码的目标图标被设置(见[配置设置](#npcbot-config-settings)),你将看到“优先目标(<角色>)...”.**优先目标为每个NPCBot单独设置**.
选择“跟随距离”将打开一个弹出窗口,你可以输入一个数值.这个数值可以在**0**到**100**之间.设置高于**100**将默认为**100**,低于**0**则为**0**.将距离设置为**0**将导致NPCBot被动跟随你,而不是主动攻击怪物,除非你攻击.

如果勾选了“禁用战斗定位”选项,你的“远程”NPCBot将不会尝试占据攻击位置,而是即使在战斗中也会保持跟随位置,只攻击能够触及的目标.

选择“攻击距离...”将允许你设置远程攻击距离.你将看到：
```
- 最小远程攻击距离
- 最大远程攻击距离
- 设定攻击距离(0-50)
- 返回
```
注：如果设置为“设定攻击距离”模式,其文本将更改为
```
- 设定攻击距离(当前设定：XX)
```
短程攻击是NPCBot职业所有远程攻击中最短距离的攻击.例如,对于圣骑士来说是10码(审判范围),对于法师来说是20码(火焰冲击范围).
长程攻击与短程攻击相反.对于大多数职业,这个范围约为30-35码.当攻击过于危险而不能靠近的目标时,这种模式很有用.
选择“精确”将打开一个弹出窗口,你可以输入一个数值.这个数值可以在**0**到**50**之间.与跟随距离的规则相同.
注：将精确攻击距离设置为**0**将使NPCBot(及其宠物)尝试在目标上方定位(忽略模型大小).

“攻击方向...”允许你设置远程机器人定位模式.你将看到：
```
- 正常
- 避免正面AOE
- 返回
```
如果你告诉NPCBot避免正面AOE,他们将尝试以一种不会被击中的方式定位自己,位于目标后面和任一侧,但只有在你这样做或者已经在目标的近战范围内时.

“参与行为...”子菜单用于管理战斗开始时机器人的行为：
```
- 延迟攻击：X.XX秒
- 治疗目标健康阈值：XX%
- 返回
```
攻击延迟是你的“DPS”NPCBot开始攻击前的等待时间(以秒为单位).**这不适用于坦克和治疗者**.
治疗阈值是单位健康百分比,当友好单位的健康值达到或低于此百分比时,将被视为NPCBot的治疗目标.这是一种告诉DPS+治疗机器人除非真的需要,否则不要治疗任何人的方式.这个参数对每个机器人都是单独的.**仅适用于治疗者**.

“优先目标(<角色>)...”允许你为每个机器人单独并按角色设置主要目标：
```
- <图标> <名称>
- <图标> <名称>
- ...
- <无>
- 返回
```
这里的“图标”是队伍目标图标(骷髅头、十字架等),“名称”是被标记单位的名称.这些行的数量是相应角色的**活动**图标的数量.如果标记的目标可以被攻击,那么在参与时,这个NPCBot将立即冲向这个单位,而不是正常的目标选择.请注意,优先目标是**按图标设置**的,并且当图标设置到另一个单位时将保持不变,这个新单位成为所有将优先级设置为此图标的机器人的新优先目标.选择“无”将为这个NPCBot禁用优先目标.

#### NPCBot 技能
NPCBots 使用大多数真实职业的法术.一些法术/技能,如增益、治疗、解除诅咒/毒药等,可以通过NPCBot的技能管理菜单获得.等级限制也适用于NPCBots,例如术士在达到8级之前无法使用恐惧.
从对话菜单选择“机器人 技能 管理...”将为您提供一个他们可以为您或您施放的法术/技能的列表.“更新”选项将刷新法术列表,因为某些法术可能当前正在冷却.
如果法术未列出,这并不意味着NPCBot没有该法术,可能只是您无法手动使用它.
NPCBots的技能检查算法包括寻找缺失的增益、需要治疗的友方、补充消耗品(如治疗石)、职业附魔(盗贼、萨满)、实用工具(如落后很远时使用疾跑)、为队伍和自身施放的法术、自我治疗、寻找群体控制目标,最后是攻击技能.

使用“管理允许的技能...”子菜单,您可以让机器人不使用它们的一些法术.禁用的法术列表会保存在数据库中.

#### NPCBot 天赋
NPCBots 不使用常规的天赋选择系统.相反,它们会使用主天赋树(根据特定职业)的同时也会选择其他两个天赋树中至第三层的关键天赋(对相同职业的玩家可用).
从对话菜单中选择“机器人 天赋 管理...”来选择一个职业.机器人将激活它,并随着你升级继续按照所选职业发展.这个操作没有冷却时间.

#### NPCBot 组队
尽管NPCBots会跟随他们的主人,无论是组队还是未组队,并且通常会为组外的人增益,但创建队伍将使NPCBots正确地使用仅为队伍成员保留的增益.
组队也是正确使用地下城寻找器所必需的(因为您不能在未组队的情况下召唤NPCBots进入或进入副本实例).

>**NOTE**:
>如果副本查找器小组内只有一名真实玩家,战利品规则将被设置为-自由拾取-

#### NPCBots 坐骑
NPCBots 会始终尝试在玩家使用载具时使用载具.对于随机类型的载具,机器人将简单地复制玩家的动作,但对于关键载具,机器人将使用自己的战术.以下是列表：
```
龙眠天翔者(永恒之眼)
红玉幼龙(魔枢)
翠绿幼龙(魔枢)
琥珀幼龙(魔枢)
银色战马/战斗座狼(冠军的试炼)
联盟/部落炮舰炮(冰冠堡垒炮舰战斗)
变异憎恶(冰冠堡垒普崔希德教授,机器人将永远不会使用)
```
注意：NPC机器人在战斗中不会自动离开载具.使用 `.npcbot vehicle eject` 命令强制它们离开载具.

#### NPCBot 附录
根据NPCBot的职业,可能在该NPCBot的对话菜单中找到额外选项.

例如,盗贼NPCBot会展示以下选项：
```
- 帮我开锁(XX)
- 我需要你刷新毒药
- <选择毒药(主手)>
- <选择毒药(副手)>
```
开锁工具允许你打开世界上的上锁箱子和你背包中的上锁物品.技能等级(XX)基于NPCBot的等级  
可以根据预期的遭遇选择毒药.当你完成时,你需要告诉NPCBot刷新毒药.

萨满NPC机器人拥有类似的武器附魔菜单

法师NPC机器人会提供给你：
```
- 我需要食物
- 我需要饮料
- 我需要一个食物桌
- 我需要一个传送门
```
这些选项将为你召唤一堆食物或饮料
如果你的等级足够高,法师NPC机器人可以为你和你的队伍召唤食物桌和传送门

术士NPC机器人将展示选项：
```
- 我需要你的治疗石
- 我需要一个灵魂之井
```
第一个选项将使术士给你他们的治疗石
如果你的等级足够高,术士NPC机器人可以召唤一个灵魂之井
这里也应用等级限制

猎人和术士NPC机器人(一旦他们达到10级)也有宠物子菜单：
```
- <选择宠物类型>
```
你仍然知道等级限制,对吧？
因为他们对猎人来说并不完全适用.他可以召唤任何类型的宠物,但异国宠物只有在80级时才会解锁

最后,所有NPC机器人都将有以下额外选项：
```
- 你被解散了
- 没事了
```
`你被解雇了!`将从你的控制中移除NPC机器人.他们会非常生气,向你扔下他们所有的装备,然后返回他们的出生位置.他们还将在5分钟内变得愤怒,以至于会攻击任何试图雇佣他们的人(这可以在配置中禁用)
`没事了`将简单地关闭对话菜单

### NPCBot 漫游系统
除了主要目的是协助玩家,NPC机器人也可以作为独立单位使用.漫游机器人配备装备,但不受玩家控制,也不能被雇佣.以下是支持的功能列表：
1. 开放世界中的漫游机器人.配置设置 **`NpcBot.WanderingBots.Continents.Count`** 控制漫游世界地图的机器人所需数量.生成点是随机的,等级相应选择.这些机器人为击杀提供小奖励和额外经验.有关更多信息,请查看配置文件.
2. 为战场生成的漫游机器人.通过 **`NpcBot.WanderingBots.BG.Enable`** 设置启用此功能,允许生成NPC机器人以填满战场队列并自己参与战场比赛.目前机器人还不能完成战场目标.目前只有战歌峡谷和阿拉希盆地被实现. 

### NPCBot Config 配置
如果某些配置设置看起来模糊不清,本节可能对您有所帮助

- **`NpcBot.BaseFollowDistance`**
  - 此参数确定队形大小和敌人追击截止距离
  - 值范围：**0-100**
  - 登出后不保存  
  解释.机器人会围绕您形成队形,其中坦克在前,近战在两侧,远程在后方.如果您设置此参数的值为30或更少,它们与您保持的距离不会改变.超过30后,它线性增加,直到您和您的机器人之间增加10码的距离.机器人开始攻击来袭敌人的距离也由此参数确定.这是*玩家*(或*机器人*,如果已部署)和敌人之间的距离,大约是此参数值的75%.如果机器人的攻击目标移出此范围,机器人将停止攻击它(除非您也攻击此目标)并撤退.**这意味着**如果此参数设置得很低,机器人的动作和追击移动可能会变得不稳定.如果此参数设置为**0**,机器人将被动行动,除非您指向攻击目标(使用您的近战攻击,只有在战斗中才有效)；这在您希望机器人攻击和撤退,或者在盲目攻击危险且您需要机器人只攻击您希望它们攻击的目标的情况下可能很有用.**自动攻击法术如自动射击或射击魔杖会取消您的近战攻击**
- **`NpcBot.XpReduction`**
  - 此参数允许您为在升级期间使用机器人的玩家设置经验值获得百分比惩罚.当组中有多于一个玩家时,用于计算经验值减少量的任何玩家在组中雇用的机器人的最大数量
  - 值范围：**0-90**  
  解释.每使用一个机器人后,获得的经验值就会减少一定百分比(无论机器人是否与玩家在组中).两个机器人能够比一个玩家造成更多的伤害,特别是在低等级时.机器人还为磨炼提供了巨大的潜力.所以您可能想要稍微惩罚您的玩家.公式是：**(100 - X * (Y - 1))%** 获得的经验值,其中**X**是经验值减少,**Y**是机器人数量.*示例：经验值减少是10,机器人数量=4；获得的经验值：100 - 10 * (4 - 1) = 70% 获得的经验值*.在任何情况下,此参数的总体经验值减少永远不会超过90%.**此惩罚只适用于机器人的所有者**
- **`NpcBot.HealTargetIconsMask`**
  - 此参数允许玩家使用目标图标将不受玩家控制的单位标记为朋友  
  解释.有时您需要保护除您自己之外的目标,护送任务是一个很好的例子.通过激活此参数,玩家可以在他们希望机器人关心的目标上设置*队伍图标*.机器人将把所说的目标视为玩家队伍的成员,如果需要就治疗它,并在战斗中协助它.参数本身是一个位掩码,由分配给每个目标图标的值组合而成：**星形 - 1,圆形 - 2,菱形 - 4,三角形 - 8,月亮 - 16,方形 - 32,十字形 - 64,骷髅 - 128**.*示例：星形、菱形和三角形 = 1 + 4 + 8 = 13*
- **`NpcBot.TankTargetIconsMask`**
  - 此参数类似于`NpcBot.HealTargetIconsMask`,但这一个为您的主要NPCBot坦克标记目标  
  您的主要坦克**不会停止攻击指向的目标**,直到它死亡或图标取消设置
- **`NpcBot.OffTankTargetIconsMask`**
  - 此参数类似于`NpcBot.TankTargetIconsMask`,但这一个为您的NPCBot副坦克标记目标  
  您的副坦克**不会停止攻击指向的目标**,直到它死亡或图标取消设置
- **`NpcBot.DPSTargetIconsMask`**
  - 与`NpcBot.TankTargetIconsMask`相同,但这一个影响您的伤害输出者,以及没有指向坦克目标且启用了伤害输出角色的(副)坦克,规则相同
- **`NpcBot.RangedDPSTargetIconsMask`**
  - 与`NpcBot.DPSTargetIconsMask`相同,但这一个只影响您的远程NPCBot
- **`NpcBot.NoDPSTargetIconMask`**
  - NPCBot将尝试不伤害这些目标
- **`Heal / Tank / DPS _TargetIconsMask`优先级**
  - 对于每种指向的目标类型,优先级从*骷髅*(最高)到*星形*(最低)
- **`Heal / Tank / DPS _TargetIconsMask`交集**
  - 如果目标图标之间有任何位掩码交集(简单来说,就是不小心或以其他方式使用了相同的图标),则应用以下规则：
    - 治疗 + 伤害输出目标**不会被**通过嘲讽或攻击攻击者来保护
    - 治疗 + 伤害输出目标**仍然可以被治疗**,如果能够被治疗
    - 治疗 + 伤害输出目标**仍然可以被攻击**,如果可攻击并且在战斗中
    - 治疗 + (副)坦克目标**仍然会被坦克**,如果能够被攻击
    - 所有伤害输出图标将自动从**`NoDPSTargetIconMask`**中排除
- **`NpcBot.Cost`**
  - 此参数确定玩家雇用机器人需要支付的金额  
  这是玩家在**80级**时需要支付的金额,对于较低等级,成本会降低：
    - 等级 1-10：成本 / 5000
    - 等级 11-20：成本 / 100
    - 等级 21-30：成本 / 20
    - 等级 31-40：成本 / 5
    - 等级 41+：成本 / 80 * 等级  
  解释.您设置的值以铜币为单位(1银币 = 100铜币,1金币 = 100银币 = 10000铜币).稀有和精英机器人的成本更高(稀有为**x2**,精英为**x5**)
- **`NpcBot.Movements.InterruptFood`**
  - 此参数确定如果机器人移动,它们是否应该停止进食和饮水  
  解释.默认情况下,此参数是禁用的,以防止机器人的食物和饮料垃圾邮件,因为机器人会抓住每一个机会进食和饮水,即使只有一秒钟不做任何事情
- **`NpcBot.EquipmentDisplay.Enable`**
  - 此参数允许机器人在它们的模型上显示除武器之外的装备物品  
  解释.通常,对于生物,游戏客户端只绘制由模型ID确定的默认模型.此参数强制客户端获取服务器端生成的单位模型和装备槽中物品的信息；因此,客户端绘制的不是默认模型,而是玩家模型组件,包括肤色、面部、面部毛发等,包括“装备”的物品.玩家可能遇到的唯一问题是来自游戏客户端的错误,这可能导致游戏退出时崩溃(客户端崩溃,不是服务器崩溃)错误#132.这个错误可以通过在很短的时间内更改具有UNIT_FLAG2_MIRROR_IMAGE的单位的基础模型超过4次来重现,因此被变形的机器人或德鲁伊变形有更高的机会造成这个问题

### NPCBot Mod 本地化
所有可本地化的字符串都包含在`npc_text`表中.如果你想进行翻译,你将不得不相应地填充`npc_text_locale`表(`Text0_0`字段).

### NPCBot 附加职业
#### 基本介绍
NPCBot 模组特色在于几个受魔兽争霸III启发的自定义职业.这些机器人被分为稀有、精英或稀有精英等级,拥有不同的法力增长速率,且不能通过饮用来恢复法力,等级和雇佣成本有所提升,可能还有最低玩家等级要求.此外,控制魔法对它们的影响要小得多,甚至比对玩家的影响还要小.它们并不旨在像普通职业那样有效或在任何特定等级上保持平衡.它们的主要目的是支持你和其他机器人.有关某些职业的基本信息,请使用对话菜单并点击“<职业介绍>”.如果你想知道更多,请继续阅读.  

#### 剑圣
*(在上一个版本中禁用)*
**等级：稀有**
**等级加成：+1**
**最低玩家等级：1**
**装备影响外观：否**
**包含数量：1**
**职业特性：** 极高的攻击速度,装备的武器不影响攻击速度,攻击力来自属性：敏捷x9,普通攻击不能暴击,不能躲避或招架,额外护甲穿透80%
可装备武器：双手剑、双手斧和长柄武器
可装备护甲：任意(不包括盾牌)
技能：

- 疾风步: 剑圣变得隐形并加快移动速度,持续30秒.如果剑圣打破隐形进行攻击,他的第一次攻击将造成150%的伤害.
- 镜像: 剑圣消失,创造出1-3个自己的幻象(取决于英雄的等级).同时移除其身上的所有魔法效果.镜像不会造成实际伤害,并且自身承受200%的伤害.
- 致命一击(被动).有15%的几率造成普通伤害的2-4倍的暴击伤害(取决于英雄的等级).
- 剑刃风暴(未实现)

**额外信息：** 剑圣作为一个职业,以其最高的单一目标伤害技能和几乎一击必杀大多数目标的技能而脱颖而出,如果装备得当的话.不幸的是,由于移动机制的变化,剑圣的暴击动画不再能够被模拟,因此这个职业在上一个版本中被禁用了.


#### 歿境神蚀者
**等级：珍稀精英**  
**等级加成：+3**  
**最低玩家等级：60**  
**装备影响外观：仅限武器**  
**包含数量：2**  
**职业特性：** *非常高* 的抗性,*负面* 法力回复,不能通过被动法力回复效果(如装备 mp5 属性)抵消,不能骑乘,无近战攻击,耐力加成 +50%,护甲加成：+50%,受到的所有伤害减少 33%,属性攻击力：力量 x2,法术强度加成：50% 攻击力 + 200% 智力 + 法杖,不能吃食  
可装备武器：法杖 **x 2**  
可装备护甲：锁甲/板甲(无盾牌),**无饰品或披风**  
技能：

- 吞噬魔法: 从敌人身上驱散多达 2 个魔法效果,从友方身上驱散多达 2 个魔法效果和 2 个诅咒效果,并在 20 码范围内对召唤单位造成伤害,恢复施法者的法力和生命值(每个驱散效果恢复 20% 法力和 5% 生命值)
- 暗影爆炸.强化攻击,对较大范围内的目标造成增加的溅射伤害.主要目标受到的伤害比其他目标更多
- 吸取法力: 从随机一个 ***友方*** 单位吸取所有法力.吸取的量受限于歿境神蚀者的最大法力值.**此技能无法禁用**
- 法力再生: 为 25 码内所有周围的队伍和队伍成员补充 3% 的最大法力.*此技能消耗所有法力*
- 生命再生: 为 25 码内所有周围的队伍和队伍成员治疗 3% 的最大生命值.*此技能消耗所有法力*
- 暗影护甲(被动): 恢复相当于受到伤害百分比的法力.此技能仅在受到的伤害足以恢复歿境神蚀者 10% 法力时触发  

**额外信息：** 歿境神蚀者实际上源自一个非英雄单位,但由于有机会造成惊人的伤害量,支持任何类型的队伍,驱散,净化并且同时承担坦克角色,利用原始单位的两种形态的技能：雕像形态和真形态

#### 高阶法师
**等级：稀有**  
**等级加成：+1**  
**最低玩家等级：20**  
**装备影响外观：否**  
**包含数量：1**  
**职业特定：** 始终骑乘(仅限地面坐骑),无近战攻击,护甲加成：智力500%,受到的法术伤害减少35%,法术强度加成：智力100%  
可装备武器：法杖  
可装备护甲：布甲(无盾牌)  
技能：

- 暴风雪: 看起来像是典型的法师暴风雪,但实际上略有不同.它没有冰冻效果,但基础伤害更高,且与法术强度的关联更大
- 召唤水元素: 大法师水元素持续时间为1分钟,冷却时间为20秒,不会耗尽法力,并且由于W3大法师可以同时激活3个元素,因此造成的伤害更多
- 光辉光环: 增加队伍和队伍成员在40码内的最大法力值10%,法力回复速度提高100%
- 群体传送(未实现)  

**附加信息：** 大法师因其在大型队伍中的辅助技能而备受重视,尽管他非常脆弱

#### 恐惧魔王
**等级：稀有精英**  
**等级加成：+3**  
**最低玩家等级：60**  
**装备影响外观：否**  
**包含数量：5**  
**职业特性：** 高抗性,不能骑乘,耐力加成+20%,护甲加成：+50%,受到的所有伤害减少15%,暴击等级x2,来自属性的攻击强度：力量x8,法术强度加成：200%力量  
可装备武器：斧头,锤,剑,双手斧,双手锤,双手剑,长柄武器,法杖,拳套,匕首  
可装备护甲：板甲(无盾牌)  
技能：

- 食腐虫群: 这个技能对一个非常大的长锥形区域内的敌人造成伤害,魔王根据造成有效伤害的100%回复生命值,可以轻松恢复魔王的全部生命,法力消耗高.食腐虫群对被控制目标造成双倍伤害
- 沉睡: 使敌人沉睡60秒,持续期间护甲减少100%.持续伤害效果不会打断此效果,只有直接伤害才会
- 吸血光环: 提高物理暴击伤害5%,为40码内的队伍和队伍成员通过近战物理攻击造成的伤害的25%(对魔王为100%)进行治疗.此治疗效果不产生威胁
- 地狱火: 在目的地召唤一个地狱仆从,对区域内的敌方单位造成伤害并使其昏迷.地狱仆从对魔法有很高的抵抗力,其属性与魔王的属性相匹配.地狱仆从燃烧,每2秒对周围敌人造成伤害,持续180秒  

**额外信息：** 在有很多近战职业的大型队伍中,魔王最有用,如果装备了足够的急速和护甲穿透,在PVP中也可能非常烦人

#### 破法者
**等级：稀有**  
**等级加成：+1**  
**最低玩家等级：20**  
**装备影响外观：否**  
**包含数量：5**  
**职业特性：** 所有法术伤害减少75%,护甲惩罚：-30%,来自属性的攻击力：力量x5,格挡几率+90%,法术强度加成：200%力量  
可装备武器：斧头、锤、剑、拳套、匕首  
可装备护甲：锁甲/板甲  
技能：

- 魔法偷取: 从敌人那里偷取有益法术并转移给附近的盟友,或从盟友那里移除负面法术并转移给附近的敌人,影响魔法和诅咒效果.转移效果的持续时间限制为最多5分钟*和最少5秒*
- 能量窃取(被动): 近战攻击燃烧目标的法力造成奥术伤害.燃烧量等于造成的近战伤害增加法术强度.如果目标被抽干,破法者的近战攻击将造成三倍伤害,并增加暴击几率.如果目标没有法力,则不消耗破法者的伤害,而是恢复他自己的法力,等于造成的伤害的25%
- 控制魔法(未实现)  

**额外信息：** 破法者主要是纯粹的辅助职业,无法造成严重伤害,但也可能迅速击败一些不幸的施法者,几秒钟内烧光他所有的法力.

#### 黑暗游侠
**等级：稀有精英**  
**等级加成：+3**  
**最低玩家等级：40**  
**装备影响外观：否**  
**包含数量：5**  
**职业特性：** 亡灵,造成伤害不产生威胁,静止时潜行模式,受到的法术伤害减少35%,来自属性的攻击力：敏捷 x4 + 智力 x2,护甲穿透加成：50%,暴击加成 +20%,闪避加成 +30%,法术强度加成：50% 智力  
可装备武器：剑、匕首、弓  
可装备护甲：布甲/皮甲(无盾牌)  
技能：

- 沉默: 使一个敌人和附近目标沉默8秒.最多5个目标,冷却时间15秒
- 黑箭: 射出一支诅咒之箭,造成150%武器伤害和额外的持续伤害.如果受影响的目标死于暗夜游侠的伤害,将从尸体中召唤出暗夜仆从(不适用于玩家),留下一堆可搜刮的血迹.如果目标生命值低于20%,则造成的伤害增加5倍.只影响人类、野兽和龙类.如果目标单位的等级为稀有、精英或稀有精英,则召唤精英仆从.最多5个仆从,仆从存活80秒,并且免疫治疗效果
- 吸取生命: 从敌人身上每秒吸取生命值,持续5秒,从0开始(共6次抽吸),治疗暗夜游侠吸取量的200%
- 魅惑(未实现)
- 嘲讽(暗夜仆从): 使近战范围内的敌人攻击暗夜仆从而不是暗夜游侠,持续5秒.一次性使用
- 改进格挡(暗夜仆从): 增加格挡攻击的几率60-100%(取决于施法者的等级),持续6秒.一次性使用  

**额外信息：** 暗夜游侠最适合与治疗者一起单独使用,或作为大型队伍中的支援角色,因为沉默的冷却时间低,且没有其他法力消耗.

#### 死灵法师
**等级：精英**  
**等级加成：+2**  
**最低玩家等级：20**  
**装备影响外观：否**  
**包含数量：5**  
**职业特性：** 受到的法术伤害减少20%,无近战攻击,法术强度加成：智力的100%  
可装备武器：法杖  
可装备护甲：布甲(无盾牌)  
技能：

- 死灵复生: 从尸体中召唤2个骷髅(最多6个骷髅,持续65秒,仅对人类、野兽和龙类有效)
- 邪恶狂热: 增加目标的近战攻击速度75%,但会持续消耗生命值,可能致命.持续45秒.无法取消.等级30解锁
- 尸体爆炸: 使一具尸体爆炸,对周围所有敌人造成相当于死者最大生命值35%至75%的伤害(取决于亡灵巫师的等级).此伤害不会产生威胁.等级40解锁
- 致残: 减少目标的移动速度、近战攻击速度和总力量50%,持续60秒.等级50解锁
- 嘲讽(骷髅): 嘲讽一个近战范围内的敌人攻击骷髅而不是亡灵巫师,持续5秒.一次性使用
- 改进格挡(骷髅): 增加格挡攻击的几率60-100%(取决于施法者的等级),持续6秒.一次性使用  

**额外信息：** 亡灵巫师带有一些《暗黑破坏神II》的精神.诅咒可能会在未来加入.亡灵巫师主要是一个辅助职业,除了尸体爆炸外,他没有任何伤害技能,但骷髅加上不洁狂热可以提供相当大的帮助.

#### 娜迦深渊海巫
**等级：稀有精英**  
**等级加成：+3**  
**最低玩家等级：1**  
**装备影响外观：否**  
**包含数量：5**  
**职业特性：** 受到的法术伤害减少30%,来自属性的攻击力：敏捷x2,招架奖励+25%,法术强度奖励：智力x200%  
可装备武器：匕首,弓  
可装备护甲：布甲(无盾牌)  
技能：

- 分叉闪电: 召唤一道闪电锥形攻击敌人.根据海巫的等级,打击2到所有目标,使他们昏迷2秒.此伤害不产生威胁  
- 冰霜箭: 为箭矢注入法术霜冻,造成额外伤害,减缓目标的移动、攻击和施法速度30%至70%(取决于海巫的等级)  
- 法力护盾: 创建一个护盾,使用海巫的法力吸收100%的入站(未减轻的)伤害.根据海巫的等级,从1点伤害每10点法力吸收到10点伤害每1点法力  
- 龙卷风: 召唤一个猛烈的龙卷风,对附近的敌方单位造成伤害并减缓速度,有时完全使它们失去技能.龙卷风在户外随时间增长,增加造成的伤害和作用范围,但在室内缩小,迅速消散.在60级解锁  
- 娜迦(被动): 游泳速度提高200%,在水里时伤害和躲避几率大幅增加  

**额外信息：** 海巫是一个多功能的远程伤害输出者.她非常耐打,并且有一些控制技能

#### 地穴领主
**等级：稀有精英**  
**等级加成：+3**  
**最低玩家等级：1**  
**装备影响外观：否**  
**包含数量：5**  
**职业特定属性：** 近战伤害减少30%,法术伤害减少15%,耐力加成+20%,护甲加成：最高+50%,属性提供的攻击力：力量x9,法术能量加成：200%力量  
可装备的武器：斧头,锤,剑,双手斧,双手锤,双手剑,长柄武器,法杖,拳套,匕首  
可装备的护甲：锁甲/板甲(无盾牌)  
技能：

- 穿刺: 地穴领主用他巨大的爪子猛击地面,向前方锥形区域射出尖刺,造成伤害并将敌人单位抛向空中,使他们昏迷.20级解锁.  
- 尖刺甲壳: 地穴领主的甲壳装甲增加了伤害抵抗力,并将15%至50%的伤害返还给敌方近战攻击者.  
- 腐尸甲虫: 地穴领主从一个新鲜的敌人尸体中繁殖出一只腐肉甲虫,以攻击他的敌人.甲虫是永久性的,但不会恢复生命值,并且一次只能控制6只.更高等级允许地穴领主召唤更强大的甲虫.10级解锁.  
- 蝗虫群: 地穴领主释放出20-40只(取决于地穴领主的等级)愤怒的蝗虫,它们啃咬并撕裂附近的敌方单位,降低它们的移动或攻击技能.当它们咀嚼敌人的肉体时,它们将其转化为一种物质,当它们返回时,这种物质会恢复地穴领主的生命值.40级解锁.  

**额外信息：** 地穴领主是一个出色的近战伤害输出者,并且有一点支持,他就能成为一个不错的近战坦克.更高等级还提供了适度的控制技能.地穴领主对基于毒的效果免疫(但不包括伤害)

### NPCBot 占位(待补充)
#### 数据库
NPCBot 数据存储在以下位置：

- `characters` 数据库
    - `characters_npcbot`(此模组创建)包含所有生成的 npcbots 的信息
    - `characters_npcbot_group_member`(此模组创建)包含 NPCBot 小组成员记录
    - `characters_npcbot_transmog`(此模组创建)包含 NPCBot 的幻化信息
    - `characters_npcbot_gear_storage`(此模组创建)包含每个玩家的 NPCBot 装备存储信息
    同时也会写入：
        - `item_instance`(物品所有者分配)
- `world` 数据库
    - `creature_template_outfits`(此模组创建)包含静态显示信息
    - `creature_template_npcbot_appearance`(此模组创建)包含动态显示信息
    - `creature_template_npcbot_extras`(此模组创建)包含种族和职业信息
    - `creature_template_npcbot_wander_nodes`(此模组创建)为漫游机器人的运动点
    - `creature_template`(ID 70000-71000\*)包含机器人的基础生物数据
    - `creature_equip_template`(ID 70000-71000\*)包含机器人的标准武器信息
    - `npc_text`(ID 70000-71000)机器人的对话和职业描述的自定义文本
    - `creature` 包含世界中生成的生物信息  
    \* - 最大 ID 可能会被自定义 NPCBots 创建超过

如果你想修改 NPCBots 使用的静态模板数据,你需要在 `world` 数据库中对上述表格中的特定 ID 进行调整(即 npcbot 模型、服装等).
**不要** 干预 `characters` 数据库中的 NPCBot 表格,任何包含意外损坏的 NPCBots 安装的 bug 报告将被无视.
如果你需要删除自定义创建的 NPCBot,你需要首先从世界中删除机器人(使用 `.npcbot delete` 命令；你需要先移除它们的装备,否则物品将变得无法访问).然后从 `creature_template_npcbot_extras`、`creature_template_npcbot_appearance` 和 `creature_equip_template` 表格中按条目(生物 ID)删除,最后从 `creature_template` 中删除.
如果你需要完全移除 NPCBot 模组,你需要首先删除世界中生成的每个机器人(使用 `.npcbot delete` 命令).然后删除 `characters_npcbot`、`characters_npcbot_group_member`、`characters_npcbot_transmog`、`characters_npcbot_gear_storage`、`creature_template_npcbot_extras`、`creature_template_npcbot_appearance` 和 `creature_template_npcbot_wander_nodes` 表格,并清除所有其他使用过的表格的条目(ID 70000-71000 + 可能还有更多自定义机器人条目).如果你不使用 Npc Dress Mod,`creature_template_outfits` 也可以被删除.

#### 游戏世界
机器人被视为活跃对象,并像玩家一样保持地图网格的加载
服务器加载时(在地图系统启动后)正在向世界中添加机器人

#### 新安装中包含的机器人总数：**312**

### NPCBot 插件
1.当前版本(3.3.5)有一个由NetherstormX开发的[NetherBot](https://github.com/NetherstormX/NetherBot)插件.

2.一个基于[NetherBot](https://github.com/AreChen/NetherBot)的针对简体中文化修订的版本插件.

---------------------------------------
## 更新日志

- **Version 0.25** (_14 Jun 2023_)
    - Added `.npcbot useonbot ...` commands
- **Version 0.24** (_20 May 2023_)
    - Crypt lord implementation details
- **Version 0.23** (_08 Feb 2023_)
    - Added addons info
- **Version 0.22** (_24 Dec 2022_)
    - General review with lots of fixes
- **Version 0.21** (_15 Dec 2022_)
    - Added `.npcbot sendto` command
- **Version 0.20** (_13 Dec 2022_)
    - Added new commands / cmd parameters
- **Version 0.19** (_24 Jun 2022_)
    - Added `.npcbot command walk` command
- **Version 0.18** (_22 Jun 2022_)
    - Added info on sea witch
- **Version 0.17** (_18 Jun 2022_)
    - Fixed numerous mistakes, added info on ICC vehicles
- **Version 0.16** (_02 Jan 2022_)
    - Added info on necromancer
- **Version 0.15** (_29 Mar 2021_)
    - Added info on new installation method
- **Version 0.14** (_23 Jan 2021_)
    - Added info on vehicle and dump commands
- **Version 0.13** (_20 Jan 2021_)
    - Added info on vehicles
- **Version 0.12** (_06 Jan 2021_)
    - Added info on autoloot
- **Version 0.11** (_07 Nov 2020_)
    - Added info on localization
- **Version 0.10** (_16 Jun 2020_)
    - Added info on new config settings
- **Version 0.9** (_09 Jun 2020_)
    - Added info on botgiver
- **Version 0.8** (_15 May 2020_)
    - Added info on raid group unit frames
    - Added info on new commands
    - Added info on talents
- **Version 0.7** (_04 Nov 2019_)
    - Added config disambiguation
    - Added info on extra classes
- **Version 0.5.1** (_3 Nov 2019_)
    - Update for branch time-stop-2013
- **Version 0.5** (_30 Oct 2019_)
    - Markdown style fix
- **Version 0.4** (_17 Oct 2019_)
    - Added all extra info

---------------------------------------