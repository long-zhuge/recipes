## React-markdown

[github](https://github.com/markedjs)

安装依赖

```
npm install marked --save
```

demo

```
import React from 'react';
import { Row, Col, Input } from 'antd';
import marked from 'marked';

class Test extends React.PureComponent {
  state = {
    string: '',
  };

  onChange = (e) => {
    this.setState({ string: e.target.value });
  };

  render() {
    return(
      <Row>
        <Col span={12}>
          <Input.TextArea style={{ height: 600 }} onChange={this.onChange} />
        </Col>
        <Col span={12}>
          <div dangerouslySetInnerHTML={{ __html: marked(this.state.string) }} />
        </Col>
      </Row>
    );
  }
}

export default Test;
```