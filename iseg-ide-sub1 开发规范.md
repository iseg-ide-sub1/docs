# iseg-ide-sub1 开发规范

## 项目整体架构

项目包含三个仓库：

- [virtual-me](https://github.com/iseg-ide-sub1/virtual-me)插件仓库，插件的前端部分，主要由 `typescript` 编写

- [virtualme-backend](https://github.com/iseg-ide-sub1/virtualme-backend) 插件模型仓库，插件的后端部分，主要由 `python` 编写

- [dataset](https://github.com/iseg-ide-sub1/dataset) 数据集仓库，插件收集的经过验证和清洗的数据，主要是 `json` 文件

其他说明：

- 每个仓库均包含一个 `/res` 文件夹，用于保存项目相关的资源（图片、文档、数据等）
- `docs` 仓库不再活跃，将其中的内容拆分到上述三个仓库中的 `/res/docs` 文件夹

## GitHub 分支管理规范

> 不同企业或组织往往有不同的 Git 分支管理规范，这些规范有很高的相似性，往往包含以下分支类型：
>
> - `Main` 主分支（持久分支）
> - `Develop` **开发分支**（持久分支）
> - `Feature` **新功能开发分支**（非持久分支）
> - `Release` 发布分支（非持久分支）
> - `Hotfix` 快速修复分支（非持久分支）
>
> 一些完整的管理规范可以参考：
>
> - https://www.cnblogs.com/kevin-ying/p/14329768.html
> - https://cloud.tencent.com/developer/article/1936451
>
> <img src="./img/01.png" style="zoom: 50%;" />

我们的项目实践，以下内容基于 Git 分支管理规范，但是进行了简化。

### 项目分支

我们的项目主要只包含三种分支，其中非持久分支在开发完成后即可删除，持久分支不能删除。

- `feature` 功能开发分支，**非持久**

  > 可以是新增功能，也可以是改动原有功能。每次我们要进行功能开发时，基于最新的 `develop` 分支创建 `feature` 分支，然后在该分支上开发，开发完成，并**初步测试没问题后**合并到 `main` 分支，并删除当前的 `feature` 分支

- `develop` 开发汇总分支，**持久**

  > 功能开发完成的分支合并到此处，**需要保证功能分支没有明显问题再合并**。不要直接在该分支进行功能开发。DEBUG 可以在该分支直接进行。

- `main` 主分支，**持久**

  > 这个分支只能从其他分支合并，不能在这个分支直接修改。当 `develop` 分支稳定后，将 `develop` 分支合并到该分支。**新版本使用该分支的内容进行发布**。

### 实践参考

1. **功能开发**
   - 基于 `develop` 分支创建 `feature-XXX` 分支
   - 在 `feature` 分支上开发
   - 开发好并初步测试没问题后合并到 `develop` 分支
2. **修复 BUG**
   - BUG 在 `main` 分支：基于 `main` 分支创建 `hotfix` 分支（临时分支），在该分支上修复好后再分别合并到 `main` 分支和 `develop` 分支上，然后删除 `hotfix` 分支
   - BUG 在其他分支：直接在对应分支上修复
3. 项目发布
   - 直接基于 `main` 分支发布

## 其他注意事项

1. `commit message` 往往也有一套约定俗成的规则，我们不做要求，大家感兴趣可以看看并尝试使用
2. 每次开发完成后建议写一下 `develop-log` 便于协作者了解互相的工作内容
3. 待补充