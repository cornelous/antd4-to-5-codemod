English | [简体中文](./README.zh-CN.md)

# Ant Design v5 Codemod

A collection of codemod scripts that help upgrade antd v5 using [jscodeshift](https://github.com/facebook/jscodeshift) and [postcss](https://github.com/postcss/postcss).(Inspired by [react-codemod](https://github.com/reactjs/react-codemod))

[![NPM version](https://img.shields.io/npm/v/@ant-design/codemod-v5.svg?style=flat)](https://npmjs.org/package/@ant-design/codemod-v5) [![NPM downloads](http://img.shields.io/npm/dm/@ant-design/codemod-v5.svg?style=flat)](https://npmjs.org/package/@ant-design/codemod-v5) [![Github Action](https://github.com/ant-design/codemod-v5/actions/workflows/test.yml/badge.svg)](https://github.com/ant-design/codemod-v5/actions/workflows/test.yml)

## Usage

Before run codemod scripts, you'd better make sure to commit your local git changes firstly.

```shell
# Run directly through npx
npx -p @cornelous/antd4-to-5-codemod antd5-codemod src

# Or run directly through pnpm
pnpm --package=@cornelous/antd4-to-5-codemod dlx antd5-codemod src
```

## Codemod scripts introduction

#### `v5-removed-component-migration`

Replace import for removed component in v5.

- Change `Comment` import from `@ant-design/compatible`.
- Change `PageHeader` import from `@ant-design/pro-layout`.
- Use `BackTop` from `FloatButton.BackTop`.

```diff
- import { Avatar, BackTop, Comment, PageHeader } from 'antd';
+ import { Comment } from '@ant-design/compatible';
+ import { PageHeader } from '@ant-design/pro-layout';
+ import { Avatar, FloatButton } from 'antd';

  ReactDOM.render( (
    <div>
      <PageHeader
        className="site-page-header"
        onBack={() => null}
        title="Title"
        subTitle="This is a subtitle"
      />
      <Comment
        actions={actions}
        author={<a>Han Solo</a>}
        avatar={<Avatar src="https://joeschmoe.io/api/v1/random" alt="Han Solo" />}
        content={
          <p>
            We supply a series of design principles, practical patterns and high quality design
            resources (Sketch and Axure), to help people create their product prototypes beautifully
            and efficiently.
          </p>
        }
        datetime={
          <span title="2016-11-22 11:22:33">8 hours ago</span>
        }
      />
-     <BackTop />
+     <FloatButton.BackTop />
    </div>
  );
```

#### `v5-props-changed-migration`

Change props usage from v4 to v5.

```diff
import { Tag, Modal, Slider } from 'antd';

const App = () => {
  const [visible, setVisible] = useState(false);

  return (
    <>
-     <Tag
-       visible={visible}
-     />
+     {visible ? <Tag /> : null}
      <Modal
-       visible={visible}
+       open={visible}
      />
-     <Slider tooltipVisible={visible} tooltipPlacement="bottomLeft" />
+     <Slider tooltip={{ placement: "bottomLeft", open: visible }} />
    </>
  );
};
```

#### `v5-removed-static-method-migration`

* Replace `message.warn` with `message.warning`.
* Replace `notification.close` with `notification.destroy`.

```diff
import { message, notification } from 'antd';

const App = () => {
  const [messageApi, contextHolder] = message.useMessage();
  const onClick1 = () => {
-   message.warn();
+   message.warning();
  }
  const onClick2 = () => {
-   messageApi.warn();
+   messageApi.warning();
  };

  const [notificationApi] = notification.useNotification();
  const onClick3 = () => {
-   notification.close();
+   notification.destroy();
  }
  const onClick4 = () => {
-   notificationApi.close();
+   notificationApi.destroy();
  };

  return <>{contextHolder}</>;
};
```

#### `v5-remove-style-import`

Comment out the style file import from antd (in js file).

```diff
- import 'antd/es/auto-complete/style';
- import 'antd/lib/button/style/index.less';
- import 'antd/dist/antd.compact.min.css';
+ // import 'antd/es/auto-complete/style';
+ // import 'antd/lib/button/style/index.less';
+ // import 'antd/dist/antd.compact.min.css';
```

#### `Remove Antd Less` in less file

Comment out the style file import from antd in less file.

```diff
- @import (reference) '~antd/dist/antd.less';
- @import '~antd/es/button/style/index.less';
+ /* @import (reference) '~antd/dist/antd.less'; */
+ /* @import '~antd/es/button/style/index.less'; */
@import './styles.less';

body {
  font-size: 14px;
}
```

## Development

```bash
  npm run release
  npm publish
```

## License

MIT
