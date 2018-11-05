# Sublime Text 非官方文档

本文档翻译自[Sublime Text Unofficial Documentation](http://docs.sublimetext.info/en/latest/)，这是一个为填补[Sublime Text 官方文档](https://www.sublimetext.com/docs/3/)中空白所创建的项目，要在线查看翻译完成的文档请参见[Sublime Text 非官方文档](https://sublime-text.readthedocs.io/en/latest/)。

## 贡献

我们通过[Issus](https://github.com/void-main/UnofficialDocs/issues)项接受翻译错误报告和新内容请求，并鼓励您发送Pull requests（但我们无法保证它们将始终合并）。

此存储库包括具有预定义设置的`.sublime-project`和使用Sphinx构建HTML文档的构建系统。


### 构建 (HTML 预览)

为了构建和预览文档，您需要[Sphinx](http://sphinx-doc.org/)。

    pip install sphinx

默认情况下，文档的预览将显示标准的Sphinx主题，但如果您愿意，可以安装和使用ReadTheDocs的主题：

    pip install sphinx_rtd_theme

如果这个主题可用，构建系统将提取它。

构建完成后，您可以在浏览器中打开`build/html/index.html`以查看指南。


[issues]: https://github.com/guillermooo/sublime-undocs/issues
