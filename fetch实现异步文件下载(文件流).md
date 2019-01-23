### fetch 实现异步文件下载(文件流)

```
export default function fetchBlob(url) {
  return fetch(url).then(res => res.blob().then(blob => {
    const a = document.createElement('a');
    // 获取 blob 本地文件连接 (blob 为纯二进制对象，不能够直接保存到磁盘上)
    const blobUrl = window.URL.createObjectURL(blob);
    // 获取 headers 中的文件名
    const filename = res.headers.get('Content-Disposition');
    a.href = blobUrl;
    a.download = filename;
    a.click();
    window.URL.revokeObjectURL(blobUrl);
    a.remove();
  }))
}
```