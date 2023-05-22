# 实现一个eventbus

码一下代码

```javascript
const eventCbMap = {

};

const eventBus = {
  $on(eventName, cb) {
    if (!eventCbMap[eventName]) eventCbMap[eventName] = [];
    eventCbMap[eventName].push(cb);
  },
  $emit(eventName, ...args) {
    if (!eventCbMap[eventName]) return;
    // once删除事件会导致下面循环过程中cb[]内fn前移, 所以此处复制成新数组
    const cbs = [...eventCbMap[eventName]];
    cbs.forEach((callback) => {
      callback?.(...args);
    });
  },
  $once(eventName, cb) {
    if (!eventCbMap[eventName]) eventCbMap[eventName] = [];
    const handler = (...args) => {
      cb(...args);
      this.$off(eventName, handler);
    };
    eventCbMap[eventName].push(handler);
  },
  $off(eventName, cb) {
    if (!eventCbMap[eventName]) return;
    const idx = eventCbMap[eventName].indexOf(cb);
    if (idx > -1) eventCbMap[eventName].splice(idx, 1);
  },
};

export default eventBus;

```

更进一步的，能不能封装成一个公共包？
