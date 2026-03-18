修改和构建本项目
====================================

TL;DR: 建议使用 ``uv`` 管理环境，并将 Python 版本锁定为 ``3.8``（兼容老版本 Sphinx）：``uv venv --python 3.8``，activate 后执行 ``uv pip install -r requirements.txt``。

1. 安装并确认 ``uv`` 可用（参考 `uv 官方文档 <https://docs.astral.sh/uv/>`_ ）。

2. 创建并激活虚拟环境（建议固定 Python 3.8 以保证依赖兼容性）：

   .. code-block:: bash

      uv venv --python 3.8
      source .venv/bin/activate

3. 在项目根目录通过 ``requirements.txt`` 一次性安装依赖：

   .. code-block:: bash

      uv pip install -r requirements.txt

4. :doc:`/rest-example` 是 ReST 的一些基本语法，也可以参考已完成的文档。
5. 修改之后，在项目根目录下 ``make clean && make html`` 即可在 ``build/html/index.html`` 查看本地构建的主页。请注意在修改章节目录结构之后需要 ``make clean`` 一下，不然可能无法正常更新。
6. 确认修改无误之后，将更改提交到自己的仓库，然后向项目仓库提交 Pull Request。如有问题，可直接提交 Issue 或课程微信群内联系助教。
