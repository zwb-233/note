# vue笔记

## vue状态管理插件

### pinia

* vue3使用的管理插件

#### 持久化

>在使用持久化时可能会出现持久化失效的问题,很可能的原因是在pinia还未加载完成时创造了对应实例
>比如,在外部声明存储变量,并在函数内函数访问时函数可能导致持久化无效

```ts
// 尽量不出现此情况
const selection = selectionStore()
function getDepartment(value: number) {
    return departmentDict.value[value];
}
```

### vuex

* vue2使用的管理插件
