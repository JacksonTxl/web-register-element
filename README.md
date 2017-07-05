# 通过html ShadowRoot创建自定义组件
自定义元素是一种用于创建自己的自定义HTML元素的功能。他们可以有自己的脚本行为和CSS样式。它们是Web组件的一部分，但也可以自己单独使用。<br>
启用自定义元素关键在于 Document.registerElement() 方法，此方法向浏览器注册一个新元素，该元素默认使用 HTMLElement 接口（如果您创建了类似<mytag>的标签，但不注册，它将会使用 HTMLUnknownElement接口。

# 生命周期回调（Lifecycle callbacks）
* createdCallback - 注册元素时执行
* attachedCallback - 元素插入DOM时执行
* detachedCallback - 元素被移除DOM时执行
* attributeChangedCallback - 元素的属性被增、删、改时执行
# demo
参照MDN，详情请参照https://developer.mozilla.org/zh-CN/docs/Web/Web_Components/Custom_Elements
### html 文件
```
If nothing appeared below, then your browser not supporting Custom Elements.
<x-product data-name="Ruby" data-img="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4621/ruby.png" data-url="http://example.com/1"></x-product>
<x-product data-name="JavaScript" data-img="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4621/javascript.png" data-url="http://example.com/2"></x-product>
<x-product data-name="Python" data-img="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4621/python.png" data-url="http://example.com/3"></x-product>
```
### js 文件
```
// Create a new object based of the HTMLElement prototype
var XProductProto = Object.create(HTMLElement.prototype);

// Set up the element.
XProductProto.createdCallback = function() {
    var _this=this;
    // Create a Shadow Root
    var shadow = this.createShadowRoot();

    // Create a standard img element and set it's attributes.
    var img = document.createElement('img');
    img.alt = this.getAttribute('data-name');
    img.src = this.getAttribute('data-img');
    img.width = '150';
    img.height = '150';
    img.className = 'product-img';

    // Add the image to the Shadow Root.
    shadow.appendChild(img);

    // Add an event listener to the image.
    img.addEventListener('click', function(e) {
        window.location = _this.getAttribute('data-url');
    });

    // Create a link to the product.
    var link = document.createElement('a');
    link.innerText = this.getAttribute('data-name');
    link.href = this.getAttribute('data-url');
    link.className = 'product-name';

            // Add the link to the Shadow Root.
    shadow.appendChild(link);
};

// Register the new element.
var XProduct = document.registerElement('x-product', {
    prototype: XProductProto
});
```

### css 文件
```
body {
  background: #F7F7F7;
}

x-product {
  display: inline-block;
  float: left;
  margin: 0.5em;
  border-radius: 3px;
  background: #FFF;
  box-shadow: 0 1px 3px rgba(0,0,0,0.25);
  font-family: Helvetica, arial, sans-serif;
  -webkit-font-smoothing: antialiased;
}

x-product::shadow .product-img {
  cursor: pointer;
  background: #FFF;
  margin: 0.5em;
}

x-product::shadow .product-name {
  display: block;
  text-align: center;
  text-decoration: none;
  color: #08C;
  border-top: 1px solid #EEE;
  font-weight: bold;
  padding: 0.75em 0;
}
```