## 构建一个命令行工具cli(command-line interface)
###### 以本项目为例
1. npm init -y
2. 替换package.json
```javascript
{
  "name": "hello-cli",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "bin": {
    "hello": "hello"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "chalk": "^2.4.2",
    "commander": "^2.20.0",
    "inquirer": "^6.3.1",
    "shelljs": "^0.8.3"
  }
}
```
3. npm install安装依赖
4. 新建入口文件hello(没有后缀)
```
#! /usr/bin/env node
const program = require('commander');
const inquirer = require('inquirer');
const chalk = require('chalk');
program
  .command('init')
  .alias('i')
  .description('初始化项目')
  .action(option => {
    // 该对象用于存储所有与用户交互的数据
    let config = {
      // 假设我们需要用户自定义项目名称
      projectName: null
    };
    // 使用chalk打印美化的版本信息
    console.log(chalk.default.bold('hello v1.0.0'));
    // 用于存储所有的交互步骤，例如让用户输入项目名称就是其中一个步骤
    let promps = [];
    if (config.projectName === null) {
      promps.push({
        type: 'input',
        name: 'projectName',
        message: '请输入项目名称',
        validate: input => {
          if (!input) {
            return '项目名称不能为空';
          }
          // 更新对象中属性的数据
          config.projectName = input;
          return true;
        }
      });
    }
    // 至此，与用户的所有交互均已完成，answers是收集到的用户所填的所有数据
    // 同时，这也是你开始操作的地方，这个cli工具的核心代码应该从这个地方开始
    inquirer.prompt(promps).then(async (answers) => {
      // do something here
      console.log(answers);
    });
  });
program.parse(process.argv);
```
5. npm link
```
将本地目录与全局node_modules做个软连接
```
6. 使用cli工具
```
hello init
# 或者
# hello i
```