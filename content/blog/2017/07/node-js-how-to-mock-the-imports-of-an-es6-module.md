---
title: "Node.js: How to mock the imports of an ES6 module"
date: "2017-07-18"
tags: [javascript,node-js,programming]
---

The package **mock-require** is useful if you want to mock **require** statements in Node.js. It has a simple API that allows you to mock anything, from a single exported function to a standard library. Here's an example:

**app/config.js**

```

function init() {
    // ...
}

module.exports = init;
```

**app/services/content.js**

```

import config from '../../config.js';

function load() {
    // ...
}

module.exports = load;
```

**test/services/content\_spec.js**

```

import {assert} from 'chai';
import sinon from 'sinon';
import mockRequire from 'mock-require';

describe('My module', () => {

    let module; // module under test
    let configMock;

    beforeEach(() => {
        configMock = {
            init: sinon.stub().returns("foo")
        };

        // mock es6 import (tip: use the same import path)
        mockRequire("../../config.js", configMock);

        // require es6 module
        module = require("../../../app/services/content.js");
    });

    afterEach(() => {
        // remove all registered mocks
        mockRequire.stopAll();
    });

    describe('Initialisation', () => {

        it('should have an load function', () => {
            assert.isFunction(module.load);
        });

    });

});
```
