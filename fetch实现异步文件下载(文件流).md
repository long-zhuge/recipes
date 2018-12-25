### fetch 实现异步文件下载(文件流)

```
fetch(url).then(res => res.blob().then(blob => { 
      var a = document.createElement('a'); 
      var url = window.URL.createObjectURL(blob);   // 获取 blob 本地文件连接 (blob 为纯二进制对象，不能够直接保存到磁盘上)
      var filename = res.headers.get('Content-Disposition'); 
      a.href = url; 
      a.download = filename; 
      a.click(); 
      window.URL.revokeObjectURL(url);
      a.remove();
});
```