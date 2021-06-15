#### 泛型避免输出any，让输出的类型跟泛型挂钩
- 用泛型把值约束在范围之内，内部范围只属于传递过来的参数决定
- 一来可以明确 输出，而来可以避免歧义
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
