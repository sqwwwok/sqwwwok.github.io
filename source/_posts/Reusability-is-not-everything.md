---
title: 不要盲目复用代码
categories:
  - Others
---
## 引言

开发过程中经常会碰到相同的逻辑，一般为了代码的整洁性，都会进行复用。
但是是不是所有相同的逻辑都需要复用呢？

## 实际场景

有两个商品卡片需要实现，其中一个是热点商品，一个是普通商品
两个在长相上有一些区别，大概在30%左右
热点商品多了标签，背景色，按钮等功能

+ 复用型写法I

```jsx
function ProductCard({isHot, price, productImage, productUrl}) {

  return (
    <div style={isHot ? styles.cardWithBg : {}}>
      {isHot && <Tag />}
      <span>{price}</span>
      {isHot && <Button />}
      <img src={productImage} onClick={() => Navigate.push(productUrl)} />
    </div>
  )

}
```

+ 复用型写法Ⅱ
```jsx
function CommonProductCard({price, productImage, productUrl}) {
  return (
    <div>
      <Price price={price} />
      <ProductImage image={productImage} url={productUrl} />
    </div>
  )
}

function HotProductCard({price, productImage, productUrl}) {
  return (
    <div style={styles.cardWithBg}>
      <Tag />
      <Price price={price} />
      <Button />
      <ProductImage image={productImage} url={productUrl} />
    </div>
  )
}

function Price({price}) {
  return <span>{price}</span>
}

function ProductImage({image, url}) {
  return <img src={productImage} onClick={() => Navigate.push(productUrl)} />
}
```

复用型I的代码说不上来的别扭，绝对的垃圾代码
复用型Ⅱ是经常能见到的，看起来解耦非常不错，也容易理解，但是细想：
热点商品为什么要跟普通商品的UI复用？两者本来就应该长得不一样。现在只是恰巧有某些地方是一样的，后来我说不定就改了。

技术层面复用逻辑没有问题，但是回到需求本身，这种复用是没有必要的，本来这两个东西就应该是分开的。
所以不如复制粘贴再写一遍更好：

+ 分离型写法
```jsx
function CommonProductCard({price, productImage, productUrl}) {
  return (
    <div>
      <span>{price}</span>
      <img src={productImage} onClick={() => Navigate.push(productUrl)} />
    </div>
  )
}

function HotProductCard({price, productImage, productUrl}) {
  return (
    <div style={styles.cardWithBg}>
      <Tag />
      <span>{price}</span>
      <Button />
      <img src={productImage} onClick={() => Navigate.push(productUrl)} />
    </div>
  )
}
```

## 总结

不要盲目的复用代码，即使他们在逻辑上有相同之处，也要看这种逻辑相同是否是需求所期望的，可能只是偶发性的相同。
