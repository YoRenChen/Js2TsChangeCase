#### 泛型避免输出any，让输出的类型跟泛型挂钩

```
export const arrayToTree = (data, rootPid, idStr) => {
  const tree: T[] = []
  let temp = []
  for (var i = 0; i < data.length; i++) {
    if (+data[i][pidStr] === +rootPid) {
      var obj = data[i]
      temp = arrayToTree(data, data[i][idStr], idStr, pidStr)
      if (temp.length > 0) {
        obj.children = temp
      }
      tree.push(obj)
    }
  }
  return tree
}
```
1. 用泛型把值约束在范围之内，内部范围只属于传递过来的参数决定
2. 一来可以明确 输出，而来可以避免歧义
    (x: number | string): number | string => <T extends number | string>(x: T): T
```
type arrayToTreeType = {
  children: arrayToTreeType[],
  [x: string]: any,
}
export const arrayToTree = <T extends arrayToTreeType>(data: T[], rootPid: string | number, idStr: string, pidStr: string) : T[] => {
  if (!data || !rootPid) {
    return []
  }

  const tree: T[] = []
  let temp = []
  for (var i = 0; i < data.length; i++) {
    if (+data[i][pidStr] === +rootPid) {
      var obj = data[i]
      temp = arrayToTree(data, data[i][idStr], idStr, pidStr)
      if (temp.length > 0) {
        obj.children = temp
      }
      tree.push(obj)
    }
  }
  return tree
}
```
#### 类型判断
```
export const msToDate = (msec, timestamp = true) => {
    const dateTime = timestamp ? (String(msec).length === 10 ? new Date(`${msec}000`) : new Date(msec)) : msec
}
```
1. msec 属性的类型由 timestamp 决定，所有 ts 判断时需要根据入参进行判断

```
type RealMsToDate<T> = T extends true ? number | string : Date

export const msToDate = (msec: number | string | Date, timestamp: boolean = true): string => {
    const dateTime: Date = timestamp ? (String(msec).length === 10 ? new Date(`${msec}000`) : new Date(msec)) : msec as RealMsToDate<typeof timestamp>
}
```

#### 赋值类型收窄

由于属性范围太广，导致在取值时发生不一致。
```
type repCode = any // 不可改

const CODE = {1001: 'A', 1002: 'B'}
function A(repCode: repCode) {
    if (repCode) {
        const responseCode = repCode
        CODE[responseCode]  // 报错
    }
}
```
修改：
```
1. 给 CODE 定义
declare type errorCodeType = {
  [key: number]: string;
}

2. 对 responseCode 范围进行收窄
- CODE[responseCode as keyof typeof CODE]
-  const responseCode: keyof typeof CODE = repCode
```
