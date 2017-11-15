Enzyme
=======

[![Join the chat at https://gitter.im/airbnb/enzyme](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/enzyme?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![npm Version](https://img.shields.io/npm/v/enzyme.svg)](https://www.npmjs.com/package/enzyme) [![License](https://img.shields.io/npm/l/enzyme.svg)](https://www.npmjs.com/package/enzyme) [![Build Status](https://travis-ci.org/airbnb/enzyme.svg)](https://travis-ci.org/airbnb/enzyme) [![Coverage Status](https://coveralls.io/repos/airbnb/enzyme/badge.svg?branch=master&service=github)](https://coveralls.io/github/airbnb/enzyme?branch=master)


Enzyme是一个用于React的JavaScript测试工具，它能更容易地断言，操作，并遍历您的React组件的输出。

通过模仿jQuery的DOM操作和遍历API，Enzyme的API是直观和灵活的。

从 Enzyme 2.x 或者 React < 16升级
===========

你是否在这里检查Enzyme是否与React 16相兼容?你目前使用Enzyme 2.x?太棒了!请参阅我们的[迁移指南](/docs/guides/migration-from-2-to-3.md)寻求帮助来转到支持React 16的Enzyme v3。

### [安装](/docs/installation/README.md)

你可以通过npm来简单地安装enzyme，并开始使用它。你还需要安装与当前你正在使用的React版本对应的enzyme适配器。例如，如果你使用的是React 16,你可以运行:
```bash
npm i --save-dev enzyme enzyme-adapter-react-16
```

每个适配器都可能有额外的对等依赖，您也需要安装这些依赖项。例如,`enzyme-adapter-react-16`对等依赖于`react`, `react-dom`, 和`react-test-renderer`。

目前，Enzyme具有与以下几个版本的React兼容的适配器：`React 16.x`, `React 15.x`,
`React 0.14.x` 和 `React 0.13.x`。

下面的适配器是由enzyme正式提供的，并且与React具有以下兼容性

| Enzyme 适配器包 | React 主版本兼容性 |
| --- | --- |
| `enzyme-adapter-react-16` | `^16.0.0` |
| `enzyme-adapter-react-15` | `^15.5.0` |
| `enzyme-adapter-react-15.4` | `15.0.0-0 - 15.4.x` |
| `enzyme-adapter-react-14` | `^0.14.0` |
| `enzyme-adapter-react-13` | `^0.13.0` |


最后，您需要配置enzyme来使用您希望它使用的适配器。要做到这一点，你可以使用顶级的`configure(...)`API。

```js
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

Enzyme.configure({ adapter: new Adapter() });
```

第三方适配器
=============

社区可以创建额外的(非正式的)适配器，从而使enzyme
与其他库一起工作。如果你已经制作了一个并且没有包含在下面的列表中，请随意
为这个README创建一个PR，并添加一个链接！已知的第三方适配器有:

| Adapter Package | For Library | Status |
| --- | --- | --- |
| [`preact-enzyme-adapter`](https://github.com/aweary/preact-enzyme-adapater) | [`preact`](https://github.com/developit/preact) | (work in progress) |
|[`enzyme-adapter-inferno`](https://github.com/bbc/enzyme-adapter-inferno)|[`inferno`](https://github.com/infernojs/inferno)|(work in progress)|

运行 Enzyme 测试
===========


对于您使用的测试运行器或断言库，Enzyme是不固执的，应该与所有主要的测试运行程序和断言库兼容。enzyme的文档和示例使用[mocha](https://mochajs.org/)和[chai](http://chaijs.com/)，但是您
应该能够推断出你的选择框架。

如果你对使用自定义断言和便利的函数的enzyme来测试您的反应组件感兴趣，您可以考虑使用:

* [`chai-enzyme`](https://github.com/producthunt/chai-enzyme) with Mocha/Chai.
* [`jasmine-enzyme`](https://github.com/blainekasten/enzyme-matchers/tree/master/packages/jasmine-enzyme) with Jasmine.
* [`jest-enzyme`](https://github.com/blainekasten/enzyme-matchers/tree/master/packages/jest-enzyme) with Jest.
* [`should-enzyme`](https://github.com/rkotze/should-enzyme) for should.js.
* [`expect-enzyme`](https://github.com/PsychoLlama/expect-enzyme) for expect.


[Using Enzyme with Mocha](/docs/guides/mocha.md)

[Using Enzyme with Karma](/docs/guides/karma.md)

[Using Enzyme with Browserify](/docs/guides/browserify.md)

[Using Enzyme with SystemJS](/docs/guides/systemjs.md)

[Using Enzyme with WebPack](/docs/guides/webpack.md)

[Using Enzyme with JSDOM](/docs/guides/jsdom.md)

[Using Enzyme with React Native](/docs/guides/react-native.md)

[Using Enzyme with Jest](/docs/guides/jest.md)

[Using Enzyme with Lab](/docs/guides/lab.md)

[Using Enzyme with Tape and AVA](/docs/guides/tape-ava.md)

基本用法
===========

## [浅渲染](/docs/api/shallow.md)

```javascript
import React from 'react';
import { expect } from 'chai';
import { shallow } from 'enzyme';
import sinon from 'sinon';

import MyComponent from './MyComponent';
import Foo from './Foo';

describe('<MyComponent />', () => {
  it('renders three <Foo /> components', () => {
    const wrapper = shallow(<MyComponent />);
    expect(wrapper.find(Foo)).to.have.length(3);
  });

  it('renders an `.icon-star`', () => {
    const wrapper = shallow(<MyComponent />);
    expect(wrapper.find('.icon-star')).to.have.length(1);
  });

  it('renders children when passed in', () => {
    const wrapper = shallow((
      <MyComponent>
        <div className="unique" />
      </MyComponent>
    ));
    expect(wrapper.contains(<div className="unique" />)).to.equal(true);
  });

  it('simulates click events', () => {
    const onButtonClick = sinon.spy();
    const wrapper = shallow(<Foo onButtonClick={onButtonClick} />);
    wrapper.find('button').simulate('click');
    expect(onButtonClick).to.have.property('callCount', 1);
  });
});
```

阅读完整的 [API文档](/docs/api/shallow.md)



## [完整的DOM渲染](/docs/api/mount.md)

```javascript
import React from 'react';
import sinon from 'sinon';
import { expect } from 'chai';
import { mount } from 'enzyme';

import Foo from './Foo';

describe('<Foo />', () => {
  it('allows us to set props', () => {
    const wrapper = mount(<Foo bar="baz" />);
    expect(wrapper.props().bar).to.equal('baz');
    wrapper.setProps({ bar: 'foo' });
    expect(wrapper.props().bar).to.equal('foo');
  });

  it('simulates click events', () => {
    const onButtonClick = sinon.spy();
    const wrapper = mount((
      <Foo onButtonClick={onButtonClick} />
    ));
    wrapper.find('button').simulate('click');
    expect(onButtonClick).to.have.property('callCount', 1);
  });

  it('calls componentDidMount', () => {
    sinon.spy(Foo.prototype, 'componentDidMount');
    const wrapper = mount(<Foo />);
    expect(Foo.prototype.componentDidMount).to.have.property('callCount', 1);
    Foo.prototype.componentDidMount.restore();
  });
});
```

阅读完整的 [API文档](/docs/api/mount.md)


## [静态渲染标记](/docs/api/render.md)

```javascript
import React from 'react';
import { expect } from 'chai';
import { render } from 'enzyme';

import Foo from './Foo';

describe('<Foo />', () => {
  it('renders three `.foo-bar`s', () => {
    const wrapper = render(<Foo />);
    expect(wrapper.find('.foo-bar').length).to.equal(3);
  });

  it('renders the title', () => {
    const wrapper = render(<Foo title="unique" />);
    expect(wrapper.text()).to.contain('unique');
  });
});
```

阅读完整的 [API 文档](/docs/api/render.md)


### 特性

[Enzyme 特性](/docs/future.md)


### 贡献

查看 [贡献指南](/CONTRIBUTING.md)

### In the wild

使用`enzyme`的组织和项目可以在[这里](https://github.com/airbnb/enzyme/blob/master/INTHEWILD.md)列出。

### License

[MIT](/LICENSE.md)
