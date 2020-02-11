* [实现](#%E5%AE%9E%E7%8E%B0)

### 实现

- 内部维护了两个属性：一个普通`map`和一个互斥量`mutex`，有两个构造器，一个带`mutex`一个不带`mutex`，如果传入`mutex`就将对象互斥锁赋值为传入的`mutex`，否则赋值为`this`，后续操作时，就会对方法上锁。

- `Collections.SynchronizedList`和`Collections.SynchroinzedSet`同理，也是用`synchronized`对`mutex`加锁，而`mutex`取决于构造器中传入的参数。

