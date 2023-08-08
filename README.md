<!--
 * @Description: jin
 * @Author: lijin
 * @Date: 2023-08-08 16:11:58
 * @LastEditTime: 2023-08-08 17:17:02
 * @LastEditors:
-->

# Babel 源码调试

- 源码下载：https://github.com/babel/babel.git -- 构建 -- 依赖安装

- 添加调试配置

  ```json
  {
    "type": "node",
    "request": "launch",
    "name": "Launch Program",
    "skipFiles": ["<node_internals>/**"],
    "console": "integratedTerminal",
    "program": "${workspaceFolder}\\examples\\test\\index.js"
  }
  ```

- 在源码目录里添加 ./examples/test/index.js 文件 -- 断点 -- 调试

  ```js
  // index.js
  const parser = require("@babel/parser");
  const traverse = require("@babel/traverse").default;
  const generate = require("@babel/generator").default;

  const source = `
      (async function() {
          console.log('hello guangguang');
      })();
  `;

  const ast = parser.parse(source);

  traverse(ast, {
    StringLiteral(path) {
      path.node.value = path.node.value.replace("guangguang", "dongdong");
    },
  });
  console.log("------haha------");
  const { code, map } = generate(ast, {
    sourceMaps: true,
  });

  console.log(code);
  console.log(JSON.stringify(map));
  ```
