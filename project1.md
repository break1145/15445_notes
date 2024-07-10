### Task#1-LRU-K Replacement Policy

> LRU-K算法从这个替换器中每次驱逐一个帧（frame），该帧的 *后向k距离* 是替换器中所有帧的最大值。
>
> *后退k距离*的计算方法：当前的时间戳 - 第k次访问前的时间戳。
>
> 对于历史访问次数小于k次的帧，其*后退k距离*被标记为+inf。当多个帧的*后向k距离*都被标记为+inf时，替换器会驱逐所有帧中整体时间戳最早的一个。（即，最早访问记录的时间是所有帧中最早的）
>
>  `LRUKReplacer`的最大尺寸就是缓冲池（buffer-pool）的尺寸，因为他包含着这个缓冲池管理器的所有帧的占位符。然而，在任意时刻，不是所有帧都被标记为可驱逐。 `LRUKReplacer`的尺寸代表了当前标记为可驱逐的帧数量，初始化时 `LRUKReplacer`内部没有任何帧。仅当有帧被标记为可驱逐时， `LRUKReplacer`的尺寸才会增加。

需要实现：

- `Evict(frame_id_t* frame_id)`: 按照`LRU-K`算法，从该`LRUKReplacer`的可驱逐帧中驱逐一个后向k距离最长的帧。驱逐成功，返回True，否则False

  - 实现思路：
    1. 找到所有可被驱逐的帧
    
    2. 找到这些帧中 后向k距离最长的
    
    3. 选择后向k距离最长的帧，删除
    
       - 线程安全：
    
         运行时应获取`LRUKReplacer`的锁

- `RecordAccess(frame_id_t frame_id)`: 记录在当前时间戳访问的帧 ID。在 `BufferPoolManager` 中固定页面后，应调用此方法。

  - 实现思路

    1. 接收参数 `frame_id`，更新`node_store_`内对应的`LRUKNode`：向`LRUKNode.history`内添加当前时间戳，如果该id不存在，就创建一个新`LRUKNode`

    - 线程安全：

      运行时对自身进行修改，应获取`LRUKReplacer`的锁

  - 排行榜（leaderboard）：

    - 添加参数`AccessType access_type = AccessType::Unknown`。

      **pass**

- `Remove(frame_id_t frame_id) `: 清楚关于某个帧的所有访问历史，仅当该帧被删除时调用

- `SetEvictable(frame_id_t frame_id, bool set_evictable)`设置指定id的可驱逐值为对应值

- `Size()`

  

  