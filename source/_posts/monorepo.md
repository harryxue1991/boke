---
title: monorepo
date: 2022-10-02 21:36:16
tags:
---


Monorepo 是一种项目开发与管理的策略模式，它代表"单一代码仓库"（Monolithic Repository）。在 Monorepo 模式中，所有相关的项目和组件都被存储在一个统一的代码仓库中，而不是分散在多个独立的代码仓库中，这些项目之间还可能会有依赖关系。

Monorepo （单仓多模块）开发模式
- 回归单体管理：Monorepo 是一种试图回归单体管理优势的方法，但保留了多仓库开发的某些优点。它允许在一个代码库中管理多个项目、组件或服务，提供更好的代码共享和重用性。
- 现代工具支持：现代的版本控制系统和工具链使得 Monorepo 开发模式更为可行，例如像 Pnpm、Yarn 、Lerna 和 Turborepo 等工具，它们提供了更好的管理、构建和部署多个项目的能力。
- 优点：

- 保留 multirepo 的主要优势
<!-- more -->

i.代码复用
ii.模块独立管理
iii.分工明确，业务场景独立
iv.代码耦合度降低
- 管理所有项目的版本控制更加容易和一致，降低了不同项目之间的版本冲突。
- 可以统一项目的构建和部署流程，降低了配置和维护多个项目所需的工作量。
- 缺点：

- Monorepo 可能随着时间推移变得庞大和复杂，导致构建时间增长和管理困难，git clone、pull 的成本增加。
- 权限管理问题：项目粒度的权限管理较为困难，容易产生非owner管理者的改动风险。
5.如何解决monorepo无法进行细粒度权限管理的缺点
1.  使用代码所有权文件使用如 CODEOWNERS 文件（GitHub 等平台支持）来指定某个目录或文件的所有者。当这些文件或目录被修改时，只有指定的所有者才能批准更改。这种方法能够实现对项目或模块级别的权限粒度控制。
2.  利用CI/CD流程在持续集成/持续部署（CI/CD）流程中设置权限和访问控制。例如，可以配置流程，只允许具有特定权限的用户触发构建或部署到生产环境。这种方式可以在流程层面上控制谁可以对代码库进行重要操作。
3.  分支策略通过严格的分支管理策略，如Git Flow，控制不同级别的开发人员可以访问和修改的分支。比如只允许项目负责人合并代码到主分支，而其他开发人员只能在特定的功能分支上工作。
4.  使用Git钩子配置Git钩子（Hooks），在代码提交或合并前执行脚本来检查提交者的权限。例如，可以设定pre-commit钩子，确保提交的代码符合访问权限要求。
5.  利用子模块虽然这种做法在传统Monorepo中较少使用，但通过Git子模块（submodules）可以实现对特定部分的仓库独立控制，从而在需要时提供更细粒度的权限管理。
6.  第三方工具和扩展考虑使用一些第三方工具和扩展来管理权限。例如，GitLab和Bitbucket等平台提供了更细粒度的权限控制设置，允许在项目或组织级别进行详细的访问控

6.为什么组件库项目会选用 Monorepo 模式
对于组件库项目，很自然的会涉及到划分以下模块
- components 包，作为组件库的主要代码，实现各个 UI 组件的核心逻辑。
- shared 包，主要存放各种杂七杂八的工具方法。
- theme 包，实现组件库的主题样式定制方案。
- cli 包，实现组件库模板脚手架的命令行工具。
- docs 包，组件库的示例 demo 与使用文档。
- playground 包，组件库的在线编辑、演示应用。
细化拆分不同模块的好处非常明显，一句话总结就是：模块划分的越清晰，复用时的灵活性、可操作性就越强，每个独立模块产物的体积也会越轻量。

pnpm add @pnpm-monorepo-demo/components --workspace
以上
@pnpm-monorepo-demo/components为
components目录下的
package.json文件中的
name属性名称，--workspace表示从工作空间安装包，也就是我们在
pnpm-workspace.yaml文件下配置的工作空间。


使用方法：
在工作空间根目录components公共组件包下创建Panel/Panel.vue组件。
这里说一下我推荐的用法小技巧，由于公共组件的包名有些长，我们可以在当前项目根目录的components目录下创建一个index.ts文件，用来导出公共组件包中的所有组件。当然你也可以根据自己的习惯来。
/projetc-a/components/index.ts
ts 代码解读复制代码
import Panel from '@pnpm-monorepo-demo/components/Panel/Panel.vue'

export {
  Panel
}


以上步骤完成后，我们就可以在页面中使用安装的公共组件包了！
App.vue
js 代码解读复制代码
<script setup lang="ts">
import { Panel } from './components'
</script>

<template>
  <Panel></Panel>
</template>

