## xml2json

> xml 和 json 的相互转化

- [github](https://github.com/rranauro/ObjTree)

npm 安装依赖

```
npm install objtree --save
```

### Example

```
import ObjTree form 'objtree';

const parser = new ObjTree();

// to json
parser.parseXML(xml);

// to xml
parser.writeXML(json);
```