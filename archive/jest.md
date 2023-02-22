# jest学习指东【draft版】

### 一些资料
[官方api文档](https://jestjs.io/zh-Hans/docs/mock-function-api)

[官方example](https://github.com/facebook/jest/tree/main/examples)

[Jest 模拟技术在wangEditor中的实践](https://juejin.cn/post/6920890656453820429#heading-3)

[jest实践开源小书](https://github.yanhaixiang.com/jest-tutorial/basic/test-environment/#%E4%BE%8B%E5%AD%90)

[jest测试异步程序](https://juejin.cn/post/7155882351275278349)

[jest模拟函数](https://jestjs.io/zh-Hans/docs/mock-functions)

[测试覆盖率](https://juejin.cn/post/7034883735732355079)

[社区](https://github.com/jest-community/awesome-jest)

### 一些笔记

#### 1、一个经典的错误❌：
```typescript

export const asyncFunction = (callback, ms = 2000) => {
  setTimeout(() => {
    callback("A Message");
  }, ms);
};
```
```typescript
// 错误代码
import { asyncFunction, fetchData } from "@/utils/data";

test("setTimeout callback", () => {
  asyncFunction((msg) => {
    expect(msg).toBe("A Message"); // 提前结束，不会执行
  });
});
```
```typescript
// 正确代码
import { asyncFunction, fetchData } from "@/utils/data";

test("setTimeout callback", (done) => {
  asyncFunction((msg) => {
    expect(msg).toBe("A Message");
    done(); // 会一直直行到done才结束；
  });
});
```

#### 2、强制指定断言执行的数量

解决的方法是使用`expect.assertions(1)`，它表示的意思是必须执行一次expect方法才可以通过测试。
```typescript
// 官方示例
test('the fetch fails with an error', () => {
  expect.assertions(1);
  return fetchData().catch(e => expect(e).toMatch('error'));
});
```

#### 3、全局设定
> 一个经典问题
```typescript
beforeAll(() => console.log('1 - beforeAll'));
afterAll(() => console.log('1 - afterAll'));
beforeEach(() => console.log('1 - beforeEach'));
afterEach(() => console.log('1 - afterEach'));

test('', () => console.log('1 - test'));

describe('Scoped / Nested block', () => {
  beforeAll(() => console.log('2 - beforeAll'));
  afterAll(() => console.log('2 - afterAll'));
  beforeEach(() => console.log('2 - beforeEach'));
  afterEach(() => console.log('2 - afterEach'));

  test('', () => console.log('2 - test'));
});

// 1 - beforeAll
// 1 - beforeEach
// 1 - test
// 1 - afterEach
// 2 - beforeAll
// 1 - beforeEach
// 2 - beforeEach
// 2 - test
// 2 - afterEach
// 1 - afterEach
// 2 - afterAll
// 1 - afterAll
```

#### 4、[模拟模块](https://jestjs.io/zh-Hans/docs/mock-functions#%E6%A8%A1%E6%8B%9F%E6%A8%A1%E5%9D%97)

全部模拟：
```javascript
import axios from 'axios';
import Users from './users';

jest.mock('axios');

test('should fetch users', () => {
  const users = [{name: 'Bob'}];
  const resp = {data: users};
  axios.get.mockResolvedValue(resp);

  // or you could use the following depending on your use case:
  // axios.get.mockImplementation(() => Promise.resolve(resp))

  return Users.all().then(data => expect(data).toEqual(users));
});
```
部分模拟：
```javascript
//test.js
import defaultExport, {bar, foo} from '../foo-bar-baz';

jest.mock('../foo-bar-baz', () => {
  const originalModule = jest.requireActual('../foo-bar-baz');

  //Mock the default export and named export 'foo'
  return {
    __esModule: true,
    ...originalModule,
    default: jest.fn(() => 'mocked baz'),
    foo: 'mocked foo',
  };
});

test('should do a partial mock', () => {
  const defaultExportResult = defaultExport();
  expect(defaultExportResult).toBe('mocked baz');
  expect(defaultExport).toHaveBeenCalled();

  expect(foo).toBe('mocked foo');
  expect(bar()).toBe('bar');
});
```
似乎更简单的写法
```typescript
// 修改前
jest.mock('node-fetch');
import fetch, {Response} from 'node-fetch';
// 修改后
jest.mock('node-fetch');
import fetch from 'node-fetch';
const {Response} = jest.requireActual('node-fetch');
```

#### 5、模拟函数
一个有趣的例子：
```javascript
const myMockFn = jest
  .fn(() => 'default')
  .mockImplementationOnce(() => 'first call')
  .mockImplementationOnce(() => 'second call');

console.log(myMockFn(), myMockFn(), myMockFn(), myMockFn());
// > 'first call', 'second call', 'default', 'default'
```

#### 6、模拟计时器


#### 7、spyon的用法

#### 8、如何测试reject和resolve;
```javascript
it('works with resolves', () => {
  expect.assertions(1);
  return expect(user.getUserName(5)).resolves.toBe('Paul');
});
// 用`.rejects`.来测试一个异步的错误
it('tests error with rejects', () => {
  expect.assertions(1);
  return expect(user.getUserName(3)).rejects.toEqual({
    error: 'User with 3 not found.',
  });
});
// 或者与async/await 一起使用 `.rejects`.
it('tests error with async/await and rejects', async () => {
  expect.assertions(1);
  await expect(user.getUserName(3)).rejects.toEqual({
    error: 'User with 3 not found.',
  });
});

// 一个和异常结合的例子
// 待测试项
const callback = asynnewc (data, error) => {
  // 判断回调是否是当前的
  clearTimeout(timer);
  this.unregister(handlerName, callbackHandler);
  if (error) {
    reject(new Error(error));
  } else {
    resolve(data);
  }
};
callback(null, new Error(error));
// 测试用例
await expect(callback(null, 'wsctest error')).rejects.toEqual(Error('wsctest error'));
```


#### 9、如何测试异常处理
参考：https://juejin.cn/post/7125708324426743838
```javascript
expect.assertions(1)
try {
  await jssdkInstance.login(data);
} catch(error) {
  expect(error).toEqual(Error('login param "userId" is required'));
}
```

#### 10、如何模拟类中的某个方法
直接mock类的原型方法

```javascript
jest.spyOn(jssdk.prototype, 'invoke');
jssdk.prototype.invoke.mockReturnValue(Promise.resolve({
  name: 'test name',
  id: '12345'
}));
```




### 一些ui级别的测试方法：

【待补充】
 

### 一些提高覆盖率的技巧：
#### a. 使用describe.each或test.each
`test.each(table)(name, fn, timeout)`

```javascript
test.each([
  [1, 1, 2],
  [1, 2, 3],
  [2, 1, 3],
])('.add(%i, %i)', (a, b, expected) => {
  expect(a + b).toBe(expected);
}); 
```

#### b. .mock对象

`mockFn.mock.calls`返回一个二维数组，mock.calls[0]表示第一次调用，返回是一个args数组。mock.calls[1][2]表示第二次调用的第三个参数值。
```javascript
[
 ['arg1', 'arg2'],
 ['arg1', 'arg2'],
];
```
`mockFn.mock.results`返回一个一维数组，数组索引表示mock函数的调用次数，mockFn.mock[0]返回一个包含type和value字段的对象，返回类型type可以是‘return’或‘throw’，表示正常返回还是抛出异常。
```javascript
[
 {
   type: 'return',
   value: 'result1',
 },
 {
   type: 'throw',
   value: {
     /* Error instance */
   },
 },
 {
   type: 'return',
   value: 'result2',
 },
];
```

#### c. 通过expect().not.toThrow()来进行条件覆盖

    由于变异测试会对条件进行变异，通过会导致无法执行的代码被执行，这时会抛出异常，通过toThrow来进行测试，可以覆盖到这些变异条件。

```javascript
expect(() => instance.destory()).not.toThrow();
```

#### d. mock模块中default导出函数
```javascript
// help.js
jest.mock('./help.js', () => ({
    __esModule: true,
    default: jest.fn(),
    funcA : jest.fn(),
}));
```

#### e. mock window.localStorage
localStorage比较特殊，通过正常的方式都不能mock成功，比如说：
```javascript
// mock失败示例1,2,3,4
jest.spyOn(localStorage, "setItem"); 
jest.spyOn(localStorage.prototype, "setItem"); 
localStorage.setItem = jest.fn();
global.localStorage = { setItem: jest.fn() };

// 正确方法：
 const storageSetItemFn = jest.spyOn(window.localStorage.__proto__, 'setItem');
```

### 一些实践

### 一些问题

覆盖率是如何计算的？如何提高覆盖率？
