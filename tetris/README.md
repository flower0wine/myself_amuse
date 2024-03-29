# 1. 项目创建时间
该项目于 2024年 02月 29日 创建。

# 2. 项目介绍
俄罗斯方块这个游戏其实在实现 `FlappyBird` 和 `gobang` 时就已经有实现的想法。但是当时
因为脑子里想到的实现方法过于复杂，所以一直没有下手，而这次下手的原因是在哔哩哔哩看到渡一教育
的 `袁进` 老师的一个关于俄罗斯方块的实现的视频。

袁老师讲的方法是使用二进制的方式来表示格子，具体的细节还得看原视频，这里就不说实现的细节了。
反正听完才有茅塞顿开的感觉，感觉实现起来好简单。真不是吹的，渡一教育有点东西。

# 3. 项目解释
花了好几天时间(2024年 03月 05日), 终于是把俄罗斯方块实现了, 包括核心逻辑和界面
样式, 实现起来还是比较吃力的, 后面讲一下较详细的实现过程。

当然也不是一下就实现了，期间我还提交了好几遍，只是都使用 `git push -f` 来强制
覆盖最近的一次推送，所以才只有一次提交历史，如果不这样做，提交历史会非常难看。
有十几次了都，其中不乏修改 bug，忘记打包，忘记添加打包后的文件到暂存区等这些导致
再次提交的情况。

## 3.1. 游玩需知: 

- 按键：

  - 左箭头: 向左移动
  - 右箭头: 向右移动
  - 上箭头: 旋转
  - 下箭头: 加速下落

- 可暂停游戏。
- 可重新开始。
- 默认每消除 30 行级别加一, 每次下落时间间隔减去 150 ms。
- 开始时下落时间间隔为 800 ms, 最小时间间隔为 300ms。
- 消除一行加一分, 同时消除多行则加分为 `行数 * 10`。级别加分为 `级别 * 5`。

## 3.2. 实现
核心的实现难点，袁老师已经给出了解决方法，就是使用二进制来表示格子。

不过还有一些难点需要攻破。

- 格子旋转。
- 头部下落的格子，部分未在展示区显现。
- 下一个要下落的格子预览。
- 提高代码的可维护性（最难）。

简单说一下：

1. 格子旋转就是将一维矩阵转为二维矩阵，进行旋转，然后再转为一维矩阵。

2. 部分未在展示区展现的格子，我使用了一个变量 `startY` 来记录格子的竖直位移，
它可以是负数，也就是未在展示区展现。开始时我并不是这样子实现的，我弄了个缓存区来
存放未在展示区显现的格子，后来因为实现不了，就再想了个办法，就是目前的这个。

3. 格子预览，这个使用了 `Vue3` 推荐的 `hook` 函数，我将其命名为 `useCreateGrid`，
在展示区和预览区分别调用了一次该方法，其中我还将创建的元素存放在 `gridPool` 中，
将生成的元素进行缓存，以复用来提高效率。

4. 最后就是代码的可维护性了，TS 并不能提高我们代码的可维护性，能提高代码可维护性的
只有我们自己，在代码中你可以看到我使用了 Vue 提供的 `readonly` 以防止变量被
外部修改而打破单向数据流，当然外部也有必须要修改的，所以我为外部提供了一个修改函数，
以免外部直接对内部变量进行修改。另外我还使用到了 `mitt` 来实现全局事件，其中将
有关变量提取到了一个单独的文件中，以提高代码的可维护性。
