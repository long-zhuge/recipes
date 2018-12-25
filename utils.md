#### get-cookie
```
export function getCookie(cookieName) {
  const name = `${cookieName}=`;
  const cookies = document.cookie.split(';');
  // eslint-disable-next-line
  for (let i = 0; i < cookies.length; i++) {
    const cookie = cookies[i].trim();
    if (cookie.indexOf(name) === 0) {
      return cookie.substring(name.length, cookie.length);
    }
  }

  return '';
}
```

#### set-cookie
|参数|描述|
|:--|:--|
|cname|名称，key|
|cvalue|值，value，必须是string类型|
|exdays|有效期，如：30，表示30天|
```
export function setCookie(cname, cvalue, exdays) {
	const d = new Date();
	d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
	const expires = "expires=" + d.toGMTString();
	document.cookie = cname + "=" + escape(cvalue) + "; " + expires;
}
```

#### decodeHtml
```
export function decodeHtml(str) {
  return str
    .replace(/&mdash;/g, '— ')
    .replace(/&ldquo;/g, '“')
    .replace(/&rdquo;/g, '”')
    .replace(/&lsquo;/g, '‘')
    .replace(/&rsquo;/g, '’')
    .replace(/&quot;/g, '"')
    .replace(/&#39;/g, '\'')
    .replace(/&lt;/g, '<')
    .replace(/&gt;/g, '>')
    .replace(/&amp;/g, '&');
}
```

#### 字符串url链接获取参数
```
export function getUrlParameter(name, link = window.location.href ) {
  let url = link;
  url = url.slice(url.indexOf('?'), url.length);
  const reg = new RegExp(`(^|&)${name}=([^&]*)(&|$)`);
  const r = url.substr(1).match(reg);

  if (r !== null) {
    return unescape(r[2]);
  }

  return null;
}
```

#### 获取 url 参数集合
```
// 获取 url 参数集合
export function getUrlParams(url) {
  const d = decodeURIComponent;
  let queryString = url ? url.split('?')[1] : window.location.search.slice(1);
  const obj = {};
  if (queryString) {
    queryString = queryString.split('#')[0]; // eslint-disable-line
    const arr = queryString.split('&');
    for (let i = 0; i < arr.length; i += 1) {
      const a = arr[i].split('=');
      let paramNum;
      const paramName = a[0].replace(/\[\d*\]/, (v) => {
        paramNum = v.slice(1, -1);
        return '';
      });
      const paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
      if (obj[paramName]) {
        if (typeof obj[paramName] === 'string') {
          obj[paramName] = d([obj[paramName]]);
        }
        if (typeof paramNum === 'undefined') {
          obj[paramName].push(d(paramValue));
        } else {
          obj[paramName][paramNum] = d(paramValue);
        }
      } else {
        obj[paramName] = d(paramValue);
      }
    }
  }
  return obj;
}
```

#### 字符串长度，中文算2个长度
```
export function getStrLength(str) {
  return str.replace(/[^\x00-\xff]/g, 'aa').length;
}
```

#### 生成唯一不重复ID
```
export function genNonDuplicateID(length) {
  return Number(Math.random().toString().substr(3, length) + Date.now()).toString(36);
}
```

#### delay 队列的延迟
```
export default function delay(time) {
  const timerId = `delay-${new Date().getTime()}`;
  console.time(timerId); // eslint-disable-line
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
      console.timeEnd(timerId); // eslint-disable-line
    }, time);
  });
}

// Generator 调用
fun...
yield delay(1000);
fun...

// 普通调用
delay(2000).then(callback);
```

#### 两数组判断相同项，并返回相同项
```
Array.prototype.existsSameValues = function(arr) {
  if (this instanceof Array && arr instanceof Array) {
    const newArray = [];
    this.forEach((item) => {
      arr.forEach((item2) => {
        if (item === item2) {
          newArray.push(item);
        }
      });
    });

    return newArray.length > 0 ? newArray : false;
  }

  return false;
};
```

#### Array.includes 重写，兼容各个浏览器
```
export function includes(arr, value) {
  return Array.prototype.includes ? arr.includes(value) : arr.some(el => el === value);
}
```

#### 数字千位符过滤，保留小数点后两位

```
export function toThousands(num, unit = '￥') {
  // let number = parseFloat(num);
  let number = +num;

  if (Number.isNaN(number)) {
    return `${unit}0.00`;
  }

  number = (+number.toFixed(2)).toLocaleString('en-US');

  if (/\./.test(number)) {
    const arr = number.split('.')[1];

    return arr.length < 2 ? `${unit}${number}0` : `${unit}${number}`;
  }

  return `${unit}${number}.00`;
}

// 输出
￥12,345.12
```

```
export function formatMoney(money) {
  if (money && money !== null) {
    return Number(money).toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
  }
  return 0.00;
}
```

#### 字符串按照字节数来截取，中文算2个字节，截取奇数时会有问题，＊慎用＊

```
function subString(str, n) {
  var r = /[^\x00-\xff]/g;
  var m;

  if (str.replace(r, '**').length > n) {
    m = Math.floor(n / 2);

    for (var i = m, l = str.length; i < l; i++) {
      if (str.substr(0, i).replace(r, '**').length >= n) {
        return str.substr(0, i);
      }
    }
  }

  return str;
}
```

#### 获取页面滚动条高度

```
function getScrollOffsets(w){
        w = w || window;
        if(w.pageXOffset != null) return {x:w.pageXOffset,y:w.pageYOffset};
        var d = w.document;
        if(document.compatMode == "CSS1Compat") return {x:d.documentElement.scrollLeft,Y:d.documentElement.scrollTop};
        return {x:d.body.scrollLeft,Y:d.body.scrollTop};
    }
```

#### 获取一个 5分制评分

```
function getRating(rating) {
    if(rating > 5 || rating < 0) throw new Error('数字不在范围内');
    return '★★★★★☆☆☆☆☆'.substring(5 - rating, 10 - rating );
}
```