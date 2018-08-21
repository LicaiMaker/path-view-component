# path-view-component

> 微信小程序 树形菜单替代组件：路径视图 path-view

## 项目截图

路径视图动态效果图-路径模式：mode1:
![路径视图动态效果图-路径模式：mode1](https://github.com/haishangfeie/path-view-component/blob/master/static/1.gif?raw=true)

路径视图动态效果图-路径模式：mode2:
![路径视图动态效果图-路径模式：mode2](https://github.com/haishangfeie/path-view-component/blob/master/static/2.gif?raw=true)

# API

| 参数              | 说明                                                                                                                                                                                                                                                         | 类型   | 可选值 | 默认值    |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ | ------ | --------- |
| value             | 要显示的树状数据                                                                                                                                                                                                                                             | Array  | Y      | []        |
| pathMode          | 路径模式，支持：'mode1'、'mode2'。'mode1'模式，点击路径跳转时视图显示对应标签下一级的内容,'mode2'模式，点击路径跳转时视图显示标签对应的那一层级的内容                                                                                                        | String | Y      | mode1     |
| firstFloorTxt     | pathMode 为'mode1'时生效，指定第一级标签的文本                                                                                                                                                                                                              | String | Y      | 第一级    |
| btnTxt            | 点击按钮显示的文本                                                                                                                                                                                                                                           | String | Y      | 选择      |
| unnormalizedValue | 仅在未传入 value 时生效，通过这个参数可以传入未转化为树状的数组                                                                                                                                                                                              | Array  | Y      | []        |
| fatherKey         | 设置 unnormalizedValue 各子元素表示父级的标识符的键值                                                                                                                                                                                                        | String | Y      | pid       |
| selfKey           | 设置 unnormalizedValue 各子元素表示自身的标识符的键值                                                                                                                                                                                                        | String | Y      | id        |
| rootValue         | 设置 unnormalizedValue 中 fatherKey 对应的值是什么时表示其是第一级的元素，即其没有父元素，默认时，没有该元素没有 fatherKey 字段或者该字段设置为 undefined 均认为其是第一级元素 | String | Y      | null |
| contentKey         | 设置 每级内容显示内容对应的键值，之前只能是item.title，现在可以根据需要设置 | String | Y      | title |

# 事件

| 事件名 | 说明                   | 参数                              |
| ------ | ---------------------- | --------------------------------- |
| tapBtn | 单击节点按钮触发的事件 | 返回传入该 item 的所有信息 Object |

# 使用方法

```
git clone https://github.com/haishangfeie/path-view-component.git
```

将 components 文件夹放入微信小程序项目根目录中

举例：例如是 index 页面要引入组件

> pages/index/index.json

```json
{
  "usingComponents": {
    "path-view": "/components/path-view/index"
  }
}
```

> pages/index/index.wxml

```html
<path-view value="{{value}}"
            btnTxt='选中本项'
            catchtapBtn="selThis"></path-view>
```

> pages/index/index.js

数据可以是树状的形式（利用 value 传入）

```js
Page({
  data: {
    value: [
      {
        // 可以根据需要定义其他的字段
        // 这个字段用于显示的文本
        title: 'a-一级节点',
        // 这个字段存放其下级的数据
        children: [
          {
            title: 'a-二级节点1',
            children: [
              {
                title: 'a-三级节点1',
                children: [
                  {
                    title: 'a-四级节点1'
                  }
                ]
              },
              {
                title: 'a-三级节点2'
              },
              {
                title: 'a-三级节点3'
              }
            ]
          },
          {
            title: 'a-二级节点2',
            children: [
              {
                title: 'a-三级节点1'
              },
              {
                title: 'a-三级节点2'
              }
            ]
          }
        ]
      },
      {
        title: 'b-一级节点',
        children: [
          {
            title: 'b-二级节点1',
            children: [
              {
                title: 'b-三级节点1'
              },
              {
                title: 'b-三级节点2'
              },
              {
                title: 'b-三级节点3'
              }
            ]
          },
          {
            title: 'b-二级节点2',
            children: [
              {
                title: 'b-三级节点1'
              },
              {
                title: 'b-三级节点2'
              }
            ]
          }
        ]
      }
    ]
  },
  selThis(e) {
    console.log(e.detail);
  }
});
```

也可以是带有每个元素带有父级标识符的普通数组(利用unnormalizedValue传入)

```
unnormalizedValue: [
  {
    id: 1,
    title:'1'
  },
  {
    id: 2,
    pid: 1,
    title:'2'
  },
  {
    id: 3,
    pid: 1,
    title: '3'
  },
  {
    id: 4,
    pid: 2,
    title: '4'
  },
  {
    id: 5,
    pid: 4,
    title: '5'
  },
  {
    id: 6,
    pid: 5,
    title: '6'
  },
  {
    id: 7,
    title: '7'
  }
]
```

# 补充
之前的版本中修改value/unnormalizedValue视图不会同步更新，我修改了一下代码，这个bug已经修复了。其他大部分值修改了其实是可以更新到的。不过，还是有两个值fatherKey/selfKey改变了视图不会刷新的，应该是只有这两个了，我看了一下，不过这个我不打算改了。原因是我觉得这两个值改变的场景不多，而且这两个值改变了，一般都会改变value/unnormalizedValue，如果重复触发视图更新好像也不是很合理。
我也想了一下，如果真的遇到要刷新视图有什么方法。。分享如下：
```js
// 方法一：


```