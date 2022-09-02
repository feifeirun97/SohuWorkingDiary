# 浅析React、Vue中的Diff算法

> 分享人：22届校招生许飞					
>
> 时间：2022/09/02
>
> 本文章不讲解vDom实现，mount挂载，以及render函数。只讨论三种diff算法。VNode类型不考虑component、functional-component、Fragment、Teleport。

## React-Diff

React的思路是递增法，通过对比新的列表中的节点，在原本的列表中的位置是否递增，来判断当前节点是否需要移动

### 1.实现原理

来看这样一个例子

![r1](/Users/xufei/Downloads/r1.png)

`nextList`为新的列表，`prevList`为旧列表。这个例子我们一眼能看出来，新列表是不需要进行移动的。下面我用`react`的递增思想，解释一下为什么新列表中的节点不需要移动。

我们首先遍历`nextList`，并且找到每一个节点，在`prevList`中的位置。

```jsx
function foo(prevList, nextList) {
    for (let i = 0; i < nextList.length; i++) {
        let nextItem = nextList[i];
        for (let j = 0; j < prevList.length; j++) {
            let prevItem = prevList[j]
            if (nextItem === prevItem) {

            }
        }
    }
}
```

找到位置以后，与上一个节点的位置进行对比，如果当前的位置大于上一个位置，说明当前节点不需要移动。因此我们要定义一个`lastIndex`来记录上一个节点的位置。

```JavaScript
function foo(prevList, nextList) {
    let lastIndex = 0
    for (let i = 0; i < nextList.length; i++) {
        let nextItem = nextList[i];
        for (let j = 0; j < prevList.length; j++) {
            let prevItem = prevList[j]
            if (nextItem === prevItem) {
                if (j < lastIndex) {
                    // 需要移动节点
                } else {
                    // 不需要移动节点，记录当前位置，与之后的节点进行对比
                    lastIndex = j
                }
            }
        }
    }
}
```

在上面的例子中，`nextList`每个节点在`prevList`的位置为`0 1 2 3`。每一项都要比前一项要大，所以不需要移动，这就是`react`的`diff`算法的原理。

### 2.找到需要移动的节点

在上一小节中，我们是通过对比值是否相等，查找的对应位置。但是在vdom中，每一个节点都是一个vNode，我们应该如何进行判断呢？

答案就是`key`，我们通过对每个节点的`key`进行赋值，并且让处于同一`children`数组下的`vnode`的`key`都不相同，以此来确定每个节点的唯一性，并进行新旧列表的对比。

```JavaScript
function reactDiff(prevChildren, nextChildren, parent) {
    let lastIndex = 0
    for (let i = 0; i < nextChildren.length; i++) {
        let nextChild = nextChildren[i];
        for (let j = 0; j < prevChildren.length; j++) {
            let prevChild = prevChildren[j]
            if (nextChild.key === prevChild.key) {
                patch(prevChild, nextChild, parent)
                if (j < lastIndex) {
                    // 需要移动节点
                } else {
                    // 不需要移动节点，记录当前位置，与之后的节点进行对比
                    lastIndex = j
                }
            }
        }
    }
}
```

### 3.移动节点

首先我们先明确一点，移动节点所指的节点是`DOM`节点。`vnode.el`指向该节点对应的真实`DOM`节点。`patch`方法会将更新过后的`DOM`节点，赋值给**新的**`vnode`的`el`属性。

> 为了画图方便，我们用`key`的值来表示`vnode`节点。为了行文方便，我们把`key`值为`a`的`vnode`简写为`vnode-a`，`vnode-a`对应的真实DOM节点为`DOM-A`


![](/Users/xufei/Downloads/r2.png)

我们来将上图的例子代入`reactDiff`中执行。我们遍历**新列表**，并查找`vnode`在**旧列表**中的位置。当遍历到`vnode-d`时，之前遍历在**旧列表**的位置为`0 < 2 < 3`，说明`A C D`这三个节点都是不需要移动的。此时`lastIndex = 3`, 并进入下一次循环，发现`vnode-b`在**旧列表**的`index`为`1`，`1 < 3`，说明`DOM-B`要移动。

通过观察我们能发现，只需要把`DOM-B`移动到`DOM-D`之后就可以了。也就是**找到需要移动的VNode，我们称该VNode为α，将α对应的真实的DOM节点移动到，α在`新列表`中的前一个VNode对应的真实DOM的后面。**

![](/Users/xufei/Downloads/r3.png)

在上述的例子中，就是将`vnode-b`对应的真实DOM节点`DOM-B`, 移动到`vnode-b`在新列表中的前一个`VNode`——`vnode-d`对应的真实DOM节点`DOM-D`的后面。

```Javascript
function reactDiff(prevChildren, nextChildren, parent) {
    let lastIndex = 0
    for (let i = 0; i < nextChildren.length; i++) {
        let nextChild = nextChildren[i];
        for (let j = 0; j < prevChildren.length; j++) {
            let prevChild = prevChildren[j]
            if (nextChild.key === prevChild.key) {
                patch(prevChild, nextChild, parent)
                if (j < lastIndex) {
                    // 移动到前一个节点的后面
                    let refNode = nextChildren[i - 1].el.nextSibling;
                    parent.insertBefore(nextChild.el, refNode)
                } else {
                    // 不需要移动节点，记录当前位置，与之后的节点进行对比
                    lastIndex = j
                }
            }
        }
    }
}
```

为什么是这样移动的呢？首先我们列表是`从头到尾`遍历的。这就意味着对于当前`VNode`节点来说，该节点之前的所有节点都是排好序的，如果该节点需要移动，那么只需要将DOM节点移动到前一个`vnode`节点之后就可以，因为在**新列表**中`vnode`的顺序就是这样的。

### 4.添加节点

上一小节我们只讲了如何移动节点，但是忽略了另外一种情况，就是在**新列表**中有全新的`VNode`节点，在**旧列表**中找不到。遇到这种情况，我们需要根据新的`VNode`节点生成`DOM`节点，并插入`DOM`树中。

至此，我们面临两个问题：1.如何发现全新的节点、2. 生成的`DOM`节点插入到哪里

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f7f3ae4ecace400a801b35c156e2edfc~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

我们先来解决第一个问题，找节点还是比较简单的，我们定义一个`find`变量值为`false`。如果在**旧列表**找到了`key` 相同的`vnode`，就将`find`的值改为`true`。当遍历结束后判断`find`值，如果为`false`，说明当前节点为新节点。

```javascript
function reactDiff(prevChildren, nextChildren, parent) {
    let lastIndex = 0
    for (let i = 0; i < nextChildren.length; i++) {
        let nextChild = nextChildren[i],
            find = false;
        for (let j = 0; j < prevChildren.length; j++) {
            let prevChild = prevChildren[j]
            if (nextChild.key === prevChild.key) {
                find = true
                patch(prevChild, nextChild, parent)
                if (j < lastIndex) {
                    // 移动到前一个节点的后面
                    let refNode = nextChildren[i - 1].el.nextSibling;
                    parent.insertBefore(nextChild.el, refNode)
                } else {
                    // 不需要移动节点，记录当前位置，与之后的节点进行对比
                    lastIndex = j
                }
                break
            }
        }
        if (!find) {
            // 插入新节点
        }
    }
}
```

找到新节点后，下一步就是插入到哪里了，这里的逻辑其实是和移动节点的逻辑是一样的。我们观察上图可以发现，新的`vnode-c`是紧跟在`vnode-b`后面的，并且`vnode-b`的DOM节点——`DOM-B`是已经排好序的，所以我们只需要将`vnode-c`生成的DOM节点插入到`DOM-B`之后就可以了。

但是这里有一种特殊情况需要注意，就是新的节点位于**新列表**的第一个，这时候我们需要找到**旧列表**第一个节点，将新节点插入到原来第一个节点之前就可以了。

```js
function reactDiff(prevChildren, nextChildren, parent) {
    let lastIndex = 0
    for (let i = 0; i < nextChildren.length; i++) {
        let nextChild = nextChildren[i],
            find = false;
        for (let j = 0; j < prevChildren.length; j++) {
            let prevChild = prevChildren[j]
            if (nextChild.key === prevChild.key) {
                find = true
                patch(prevChild, nextChild, parent)
                if (j < lastIndex) {
                    // 移动到前一个节点的后面
                    let refNode = nextChildren[i - 1].el.nextSibling;
                    parent.insertBefore(nextChild.el, refNode)
                } else {
                    // 不需要移动节点，记录当前位置，与之后的节点进行对比
                    lastIndex = j
                }
                break
            }
        }
        if (!find) {
            // 插入新节点
            let refNode = i <= 0
                            ? prevChildren[0].el
                            : nextChildren[i - 1].el.nextSibling
            mount(nextChild, parent, refNode);
        }
    }
}
```

## 5. 移除节点

有增就有减，当旧的节点不在**新列表**中时，我们就将其对应的DOM节点移除。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9cf836312b6345d98b4ea8c9efe80a9a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

```js
function reactDiff(prevChildren, nextChildren, parent) {
    let lastIndex = 0
    for (let i = 0; i < nextChildren.length; i++) {
        let nextChild = nextChildren[i],
            find = false;
        for (let j = 0; j < prevChildren.length; j++) {
            let prevChild = prevChildren[j]
            if (nextChild.key === prevChild.key) {
                find = true
                patch(prevChild, nextChild, parent)
                if (j < lastIndex) {
                    // 移动到前一个节点的后面
                    let refNode = nextChildren[i - 1].el.nextSibling;
                    parent.insertBefore(nextChild.el, refNode)
                } else {
                    // 不需要移动节点，记录当前位置，与之后的节点进行对比
                    lastIndex = j
                }
                break
            }
        }
        if (!find) {
            // 插入新节点
            let refNode = i <= 0
                            ? prevChildren[0].el
                            : nextChildren[i - 1].el.nextSibling
            mount(nextChild, parent, refNode);
        }
    }
    for (let i = 0; i < prevChildren.length; i++) {
        let prevChild = prevChildren[i],
            key = prevChild.key,
            has = nextChildren.find(item => item.key === key);
        if (!has) parent.removeChild(prevChild.el)
    }
}
```

### 6.优化与不足

以上就是React的diff算法的思路。

目前的`reactDiff`的时间复杂度为`O(m*n)`，我们可以用空间换时间，把`key`与`index`的关系维护成一个`Map`，从而将时间复杂度降低为`O(n)`

我们接下来看这样一个例子

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aed2a562a9674f5293e4e51a1b8b9005~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

根据`reactDiff`的思路，我们需要先将`DOM-A`移动到`DOM-C`之后，然后再将`DOM-B`移动到`DOM-A`之后，完成`Diff`。但是我们通过观察可以发现，只要将`DOM-C`移动到`DOM-A`之前就可以完成`Diff`。

这里是有可优化的空间的，接下来我们介绍`vue2.x`中的`diff`算法——`双端比较`，该算法解决了上述的问题

## 二、Vue2.X Diff —— 双端比较

所谓`双端比较`就是**新列表**和**旧列表**两个列表的头与尾互相对比，，在对比的过程中指针会逐渐向内靠拢，直到某一个列表的节点全部遍历过，对比停止。

### 1.实现原理

我们先用四个指针指向两个列表的头尾

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  let oldStartIndex = 0,
    oldEndIndex = prevChildren.length - 1
    newStartIndex = 0,
    newEndIndex = nextChildren.length - 1;
  let oldStartNode = prevChildren[oldStartIndex],
    oldEndNode = prevChildren[oldEndIndex],
    newStartNode = nextChildren[nextStartIndex],
    newEndNode = nextChildren[nextEndIndex];
}

```

我们根据四个指针找到四个节点，然后进行对比，那么如何对比呢？我们按照以下四个步骤进行对比

1. 使用**旧列表**的头一个节点`oldStartNode`与**新列表**的头一个节点`newStartNode`对比
2. 使用**旧列表**的最后一个节点`oldEndNode`与**新列表**的最后一个节点`newEndNode`对比
3. 使用**旧列表**的头一个节点`oldStartNode`与**新列表**的最后一个节点`newEndNode`对比
4. 使用**旧列表**的最后一个节点`oldEndNode`与**新列表**的头一个节点`newStartNode`对比

使用以上四步进行对比，去寻找`key`相同的可复用的节点，当在某一步中找到了则停止后面的寻找。具体对比顺序如下图

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/847306f303ab4177891b56cccff1ebd3~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

对比顺序代码结构如下:

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  let oldStartIndex = 0,
    oldEndIndex = prevChildren.length - 1
    newStartIndex = 0,
    newEndIndex = nextChildren.length - 1;
  let oldStartNode = prevChildren[oldStartIndex],
    oldEndNode = prevChildren[oldEndIndex],
    newStartNode = nextChildren[newStartIndex],
    newEndNode = nextChildren[newEndIndex];
  
  if (oldStartNode.key === newStartNode.key) {

  } else if (oldEndNode.key === newEndNode.key) {

  } else if (oldStartNode.key === newEndNode.key) {

  } else if (oldEndNode.key === newStartNode.key) {

  }
}
```

当对比时找到了可复用的节点，我们还是先`patch`给元素打补丁，然后将指针进行`前/后移`一位指针。根据对比节点的不同，我们移动的**指针**和**方向**也不同，具体规则如下：

1. 当**旧列表**的头一个节点`oldStartNode`与**新列表**的头一个节点`newStartNode`对比时`key`相同。那么**旧列表**的头指针`oldStartIndex`与**新列表**的头指针`newStartIndex`同时向**后**移动一位。
2. 当**旧列表**的最后一个节点`oldEndNode`与**新列表**的最后一个节点`newEndNode`对比时`key`相同。那么**旧列表**的尾指针`oldEndIndex`与**新列表**的尾指针`newEndIndex`同时向**前**移动一位。
3. 当**旧列表**的头一个节点`oldStartNode`与**新列表**的最后一个节点`newEndNode`对比时`key`相同。那么**旧列表**的头指针`oldStartIndex`向**后**移动一位；**新列表**的尾指针`newEndIndex`向**前**移动一位。
4. 当**旧列表**的最后一个节点`oldEndNode`与**新列表**的头一个节点`newStartNode`对比时`key`相同。那么**旧列表**的尾指针`oldEndIndex`向**前**移动一位；**新列表**的头指针`newStartIndex`向**后**移动一位。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  let oldStartIndex = 0,
    oldEndIndex = prevChildren.length - 1,
    newStartIndex = 0,
    newEndIndex = nextChildren.length - 1;
  let oldStartNode = prevChildren[oldStartIndex],
    oldEndNode = prevChildren[oldEndIndex],
    newStartNode = nextChildren[newStartIndex],
    newEndNode = nextChildren[newEndIndex];

  if (oldStartNode.key === newStartNode.key) {
    patch(oldvStartNode, newStartNode, parent)

    oldStartIndex++
    newStartIndex++
    oldStartNode = prevChildren[oldStartIndex]
    newStartNode = nextChildren[newStartIndex]
  } else if (oldEndNode.key === newEndNode.key) {
    patch(oldEndNode, newEndNode, parent)

    oldEndIndex--
    newEndIndex--
    oldEndNode = prevChildren[oldEndIndex]
    newEndNode = nextChildren[newEndIndex]
  } else if (oldStartNode.key === newEndNode.key) {
    patch(oldStartNode, newEndNode, parent)

    oldStartIndex++
    newEndIndex--
    oldStartNode = prevChildren[oldStartIndex]
    newEndNode = nextChildren[newEndIndex]
  } else if (oldEndNode.key === newStartNode.key) {
    patch(oldEndNode, newStartNode, parent)

    oldEndIndex--
    nextStartIndex++
    oldEndNode = prevChildren[oldEndIndex]
    newStartNode = nextChildren[newStartIndex]
  }
}
```

在小节的开头，提到了要让指针向内靠拢，所以我们需要循环。循环停止的条件是当其中一个列表的节点全部遍历完成，代码如下

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  let oldStartIndex = 0,
    oldEndIndex = prevChildren.length - 1,
    newStartIndex = 0,
    newEndIndex = nextChildren.length - 1;
  let oldStartNode = prevChildren[oldStartIndex],
    oldEndNode = prevChildren[oldEndIndex],
    newStartNode = nextChildren[newStartIndex],
    newEndNode = nextChildren[newEndIndex];
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode.key === newStartNode.key) {
      patch(oldStartNode, newStartNode, parent)

      oldStartIndex++
      newStartIndex++
      oldStartNode = prevChildren[oldStartIndex]
      newStartNode = nextChildren[newStartIndex]
    } else if (oldEndNode.key === newEndNode.key) {
      patch(oldEndNode, newEndNode, parent)

      oldEndIndex--
      newndIndex--
      oldEndNode = prevChildren[oldEndIndex]
      newEndNode = nextChildren[newEndIndex]
    } else if (oldStartNode.key === newEndNode.key) {
      patch(oldvStartNode, newEndNode, parent)

      oldStartIndex++
      newEndIndex--
      oldStartNode = prevChildren[oldStartIndex]
      newEndNode = nextChildren[newEndIndex]
    } else if (oldEndNode.key === newStartNode.key) {
      patch(oldEndNode, newStartNode, parent)

      oldEndIndex--
      newStartIndex++
      oldEndNode = prevChildren[oldEndIndex]
      newStartNode = nextChildren[newStartIndex]
    }
  }
}
```

至此整体的循环我们就全部完成了，下面我们需要考虑这样两个问题：

- 什么情况下`DOM`节点需要移动
- `DOM`节点如何移动

我们来解决第一个问题：**什么情况下需要移动**，我们还是以上图为例。

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d8c96a75e48346aea2c7de1d8bfa6f67~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

当我们在第一个循环时，在`第四步`发现**旧列表的尾节点**`oldEndNode`与**新列表的头节点**`newStartNode`的`key`相同，是可复用的`DOM`节点。通过观察我们可以发现，**原本在旧列表末尾的节点，却是新列表中的开头节点，没有人比他更靠前，因为他是第一个，所以我们只需要把当前的节点移动到原本旧列表中的第一个节点之前，让它成为第一个节点即可**。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
 // ...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode.key === newStartNode.key) {
       // ...
    } else if (oldEndNode.key === newEndNode.key) {
      // ...
    } else if (oldStartNode.key === newEndNode.key) {
      // ...
    } else if (oldEndNode.key === newStartNode.key) {
      patch(oldEndNode, newStartNode, parent)
      // 移动到旧列表头节点之前
      parent.insertBefore(oldEndNode.el, oldStartNode.el)
      
      oldEndIndex--
      newStartIndex++
      oldEndNode = prevChildren[oldEndIndex]
      newStartNode = nextChildren[newStartIndex]
    }
  }
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c7f3368a5a5744bcb2b356d7424093a6~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

然后我们进入第二次循环，我们在`第二步`发现，**旧列表的尾节点**`oldEndNode`和**新列表的尾节点**`newEndNode`为复用节点。**原本在旧列表中就是尾节点，在新列表中也是尾节点，说明该节点不需要移动**，所以我们什么都不需要做。

同理，如果是**旧列表的头节点**`oldStartNode`和**新列表的头节点**`newStartNode`为复用节点，我们也什么都不需要做。

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2520c8511aad44589947be3616f7f50b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

进入第三次循环，我们在`第三部`发现，**旧列表的头节点**`oldStartNode`和**新列表的尾节点**`newEndNode`为复用节点。到这一步聪明如你肯定就一眼可以看出来了，我们只要将`DOM-A`移动到`DOM-B`后面就可以了。

依照惯例我们还是解释一下，**原本旧列表中是头节点，然后在新列表中是尾节点。那么只要在旧列表中把当前的节点移动到原本尾节点的后面，就可以了**。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  // ...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode.key === newStartNode.key) {
      // ...
    } else if (oldEndNode.key === newEndNode.key) {
      // ...
    } else if (oldStartNode.key === newEndNode.key) {
      patch(oldStartNode, newEndNode, parent)
      parent.insertBefore(oldStartNode.el, oldEndNode.el.nextSibling)

      oldStartIndex++
      newEndIndex--
      oldStartNode = prevChildren[oldStartIndex]
      newEndNode = nextChildren[newEndIndex]
    } else if (oldEndNode.key === newStartNode.key) {
     //...
    }
  }
}
```

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d0a744cac8045fc9ae4f593a153cc72~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

OK，进入最后一个循环。在`第一步`**旧列表**头节点`oldStartNode`与**新列表**头节点`newStartNode`位置相同，所以啥也不用做。然后结束循环，这就是`Vue2 双端比较`的原理。

## 2. 非理想情况

上一小节，我们讲了`双端比较`的原理，但是有一种特殊情况，当四次对比都**没找到**复用节点时，我们只能拿**新列表**的第一个节点去**旧列表**中找与其`key`相同的节点。

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2dea77f7267b406ca0dc8600096c4dc1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  //...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode.key === newStartNode.key) {
    //...
    } else if (oldEndNode.key === newEndNode.key) {
    //...
    } else if (oldStartNode.key === newEndNode.key) {
    //...
    } else if (oldEndNode.key === newStartNode.key) {
    //...
    } else {
      // 在旧列表中找到 和新列表头节点key 相同的节点
      let newKey = newStartNode.key,
        oldIndex = prevChildren.findIndex(child => child.key === newKey);
      
    }
  }
}
```

找节点的时候其实会有两种情况：一种在**旧列表**中找到了，另一种情况是没找到。我们先以上图为例，说一下找到的情况。

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a74e61a47abb4e3ca85ffee65410340d~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

当我们在旧列表中找到对应的`VNode`，我们只需要将找到的节点的`DOM`元素，移动到开头就可以了。这里的逻辑其实和`第四步`的逻辑是一样的，只不过`第四步`是移动的尾节点，这里是移动找到的节点。`DOM`移动后，由我们将**旧列表**中的节点改为`undefined`，这是**至关重要**的一步，因为我们已经做了节点的移动了所以我们不需要进行再次的对比了。最后我们将头指针`newStartIndex`向后移一位。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  //...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode.key === newStartNode.key) {
    //...
    } else if (oldEndNode.key === newEndNode.key) {
    //...
    } else if (oldStartNode.key === newEndNode.key) {
    //...
    } else if (oldEndNode.key === newStartNode.key) {
    //...
    } else {
      // 在旧列表中找到 和新列表头节点key 相同的节点
      let newtKey = newStartNode.key,
        oldIndex = prevChildren.findIndex(child => child.key === newKey);
      
      if (oldIndex > -1) {
        let oldNode = prevChildren[oldIndex];
        patch(oldNode, newStartNode, parent)
        parent.insertBefore(oldNode.el, oldStartNode.el)
        prevChildren[oldIndex] = undefined
      }
      newStartNode = nextChildren[++newStartIndex]
    }
  }
}
```

如果在**旧列表**中没有找到复用节点呢？很简单，直接创建一个新的节点放到最前面就可以了，然后后移头指针`newStartIndex`。

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4bbefb16b2bf4d52bb9b2a5f5ea86ec1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  //...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode.key === newStartNode.key) {
    //...
    } else if (oldEndNode.key === newEndNode.key) {
    //...
    } else if (oldStartNode.key === newEndNode.key) {
    //...
    } else if (oldEndNode.key === newStartNode.key) {
    //...
    } else {
      // 在旧列表中找到 和新列表头节点key 相同的节点
      let newtKey = newStartNode.key,
        oldIndex = prevChildren.findIndex(child => child.key === newKey);
      
      if (oldIndex > -1) {
        let oldNode = prevChildren[oldIndex];
        patch(oldNode, newStartNode, parent)
        parent.insertBefore(oldNode.el, oldStartNode.el)
        prevChildren[oldIndex] = undefined
      } else {
      	mount(newStartNode, parent, oldStartNode.el)
      }
      newStartNode = nextChildren[++newStartIndex]
    }
  }
}
```

最后当**旧列表**遍历到`undefind`时就跳过当前节点。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  //...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
    if (oldStartNode === undefind) {
    	oldStartNode = prevChildren[++oldStartIndex]
    } else if (oldEndNode === undefind) {
    	oldEndNode = prevChildren[--oldEndIndex]
    } else if (oldStartNode.key === newStartNode.key) {
    //...
    } else if (oldEndNode.key === newEndNode.key) {
    //...
    } else if (oldStartNode.key === newEndNode.key) {
    //...
    } else if (oldEndNode.key === newStartNode.key) {
    //...
    } else {
    // ...
    }
  }
}

```

## 3.添加节点

我们先来看一个例子

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/85517a9eb0a34165832394b9d4e7627d~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

这个例子非常简单，几次循环都是尾节点相同，尾指针一直向前移动，直到循环结束，如下图

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/668208458312440cae49139534fd6d59~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

此时`oldEndIndex`以及小于了`oldStartIndex`，但是**新列表**中还有剩余的节点，我们只需要将剩余的节点依次插入到`oldStartNode`的`DOM`之前就可以了。为什么是插入`oldStartNode`之前呢？原因是剩余的节点在**新列表**的位置是位于`oldStartNode`之前的，如果剩余节点是在`oldStartNode`之后，`oldStartNode`就会先行对比，这个需要思考一下，其实还是与`第四步`的思路一样。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  //...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
  // ...
  }
  if (oldEndIndex < oldStartIndex) {
    for (let i = newStartIndex; i <= newEndIndex; i++) {
      mount(nextChildren[i], parent, prevStartNode.el)
    }
  }
}

```

## 4.移除节点

与上一小节的情况相反，当**新列表**的`newEndIndex`小于`newStartIndex`时，我们将**旧列表**剩余的节点删除即可。这里我们需要注意，**旧列表**的`undefind`。在第二小节中我们提到过，当头尾节点都不相同时，我们会去**旧列表**中找**新列表**的第一个节点，移动完DOM节点后，将**旧列表**的那个节点改为`undefind`。所以我们在最后的删除时，需要注意这些`undefind`，遇到的话跳过当前循环即可。

```js
function vue2Diff(prevChildren, nextChildren, parent) {
  //...
  while (oldStartIndex <= oldEndIndex && newStartIndex <= newEndIndex) {
  // ...
  }
  if (oldEndIndex < oldStartIndex) {
    for (let i = newStartIndex; i <= newEndIndex; i++) {
      mount(nextChildren[i], parent, prevStartNode.el)
    }
  } else if (newEndIndex < newStartIndex) {
    for (let i = oldStartIndex; i <= oldEndIndex; i++) {
      if (prevChildren[i]) {
        partent.removeChild(prevChildren[i].el)
      }
    }
  }
}
```

## 5.小结

至此`双端比较`全部完成，以下是全部代码。

```js
function vue2diff(prevChildren, nextChildren, parent) {
  let oldStartIndex = 0,
    newStartIndex = 0,
    oldStartIndex = prevChildren.length - 1,
    newStartIndex = nextChildren.length - 1,
    oldStartNode = prevChildren[oldStartIndex],
    oldEndNode = prevChildren[oldStartIndex],
    newStartNode = nextChildren[newStartIndex],
    newEndNode = nextChildren[newStartIndex];
  while (oldStartIndex <= oldStartIndex && newStartIndex <= newStartIndex) {
    if (oldStartNode === undefined) {
      oldStartNode = prevChildren[++oldStartIndex]
    } else if (oldEndNode === undefined) {
      oldEndNode = prevChildren[--oldStartIndex]
    } else if (oldStartNode.key === newStartNode.key) {
      patch(oldStartNode, newStartNode, parent)

      oldStartIndex++
      newStartIndex++
      oldStartNode = prevChildren[oldStartIndex]
      newStartNode = nextChildren[newStartIndex]
    } else if (oldEndNode.key === newEndNode.key) {
      patch(oldEndNode, newEndNode, parent)

      oldStartIndex--
      newStartIndex--
      oldEndNode = prevChildren[oldStartIndex]
      newEndNode = nextChildren[newStartIndex]
    } else if (oldStartNode.key === newEndNode.key) {
      patch(oldStartNode, newEndNode, parent)
      parent.insertBefore(oldStartNode.el, oldEndNode.el.nextSibling)
      oldStartIndex++
      newStartIndex--
      oldStartNode = prevChildren[oldStartIndex]
      newEndNode = nextChildren[newStartIndex]
    } else if (oldEndNode.key === newStartNode.key) {
      patch(oldEndNode, newStartNode, parent)
      parent.insertBefore(oldEndNode.el, oldStartNode.el)
      oldStartIndex--
      newStartIndex++
      oldEndNode = prevChildren[oldStartIndex]
      newStartNode = nextChildren[newStartIndex]
    } else {
      let newKey = newStartNode.key,
        oldIndex = prevChildren.findIndex(child => child && (child.key === newKey));
      if (oldIndex === -1) {
        mount(newStartNode, parent, oldStartNode.el)
      } else {
        let prevNode = prevChildren[oldIndex]
        patch(prevNode, newStartNode, parent)
        parent.insertBefore(prevNode.el, oldStartNode.el)
        prevChildren[oldIndex] = undefined
      }
      newStartIndex++
      newStartNode = nextChildren[newStartIndex]
    }
  }
  if (newStartIndex > newStartIndex) {
    while (oldStartIndex <= oldStartIndex) {
      if (!prevChildren[oldStartIndex]) {
        oldStartIndex++
        continue
      }
      parent.removeChild(prevChildren[oldStartIndex++].el)
    }
  } else if (oldStartIndex > oldStartIndex) {
    while (newStartIndex <= newStartIndex) {
      mount(nextChildren[newStartIndex++], parent, oldStartNode.el)
    }
  }
}
```

## 总结

+ ### react -diff

![](/Users/xufei/Downloads/r3.png)

+ ### vue -diff双端算法

  ![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2dea77f7267b406ca0dc8600096c4dc1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

实际上真实的算法会复杂很多，







比如React为了降低算法复杂度，React的diff预设三个限制：

1. 只对同级元素进行Diff。如果一个DOM节点在前后两次更新中跨越了层级，那么React不会尝试复用他。
2. 两个不同类型的元素会产生出不同的树。如果元素由`div`变为`p`，React会销毁`div`及其子孙节点，并新建`p`及其子孙节点。
3. 开发者可以通过 `key`属性 来暗示哪些子元素在不同的渲染下能保持稳定。考虑如下例子：

```javascript
// 更新前
<div>
    <p key="k">ka</p>
    <h3 key="s">song</h3>
</div>

// 更新后
<div>
    <h3 key="s">song</h3>
    <p key="k">ka</p>
</div>
```

如果没有`key`，React会认为`div`的第一个子节点由`p`变为`h3`，第二个子节点由`h3`变为`p`。这符合限制2的设定，会销毁并新建。

但是当我们用`key`指明了节点前后对应关系后，React知道`key === "ka"`的`p`在更新后还存在，所以DOM节点可以复用，只是需要交换下顺序。

这就是React为了应对算法性能瓶颈做出的三条限制。

