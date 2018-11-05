========
包
========

包只是 ``Packages`` 下的目录。它们主要用于组织目的，但Sublime Text在处理它们时遵循一些规则。稍后会详细介绍。

这是包含在包中的典型资源列表：
	
    - 构建系统 (``.sublime-build``)
    - 快捷键 (``.sublime-keymap``)
    - 宏 (``.sublime-macro``)
    - 菜单 (``.sublime-menu``)
    - 插件 (``.py``)
    - 偏好 (``.tmPreferences``)
    - 设置 (``.sublime-settings``)
    - 语法定义 (``.tmLanguage``)
    - 代码片段 (``.sublime-snippet``)
    - 主题 (``.sublime-theme``)

某些软件包可能包含其他软件包或核心功能的支持文件。例如，拼写检查器使用 :file:`Packages\Language - English` 作为英语词典的数据存储。


包的类型
*****************

为了讨论本指南中的软件包，我们将它们分组。这个划分是人为的，为了本主题的清晰起见。Sublime Text不以任何方式使用它。

**核心包**
	Sublime Text需要这些包才能工作。

**运送包**
   Sublime Text在每个安装中都包含这些包，尽管它们在技术上并不是必需的。运送包可以增强Sublime Text的功能。它们可能是由用户或第三方提供的。

**用户包**
   这些包由用户安装，以进一步扩展Sublime Text。它们不是任何Sublime Text安装的一部分，并且始终由用户或第三方提供。

**已安装包**
  Sublime Text可以在删除时恢复的任何包。

让我们再次强调一点，你不需要记住这个分类。此外，值得注意的是，*第三方* 我们主要是指其他编辑的用户，如Textmate的用户。


安装包
************************

安装包有两种主要方式：

	- ``.sublime-package`` 文件
	- 版本控制系统

最终，安装包只需要在 ``Packages`` 下放置一个包含Sublime Text资源的目录。从一个系统到另一个系统的唯一变化是如何复制这些文件。

.. sidebar:: 安装包与已安装包
	
   请注意，安装包实际上并不会使该包成为已安装包。已安装包是驻留在 *Installed Packages* 目录中的 ``.sublime-package`` 文件。在本指南中，我们使用安装包来表示将包复制到 ``Packages``。

   Sublime Text可以恢复位于 ``Installed Packages`` 中的任何包，但不能恢复位于 ``Packages`` 中的每个包。

.. _installation-of-sublime-packages:

安装 ``.sublime-package`` 文件
------------------------------------------

将 ``.sublime-package`` 文件复制到 ``Installed Packages`` 目录并重新启动Sublime Text。如果 ``Installed Packages`` 不存在，则可以创建它。

请注意，``.sublime-package`` 文件只是带有自定义文件扩展名的 ``.zip`` 文件。

从版本控制系统安装包
------------------------------------------------------

解释如何使用版本控制系统（VCSs）超出了本指南的范围，但是在Google Code，GitHub和Bitbucket等公共存储库上有许多免费的用户包可用。

此外，GitHub上还有一个向贡献者开放的 `Sublime Text组织`_。

.. _Sublime Text组织: http://github.com/SublimeText


包与魔法
******************

Sublime Text处理包的方式中几乎没有隐形魔法。两个值得注意的例外是任何包中定义的宏都出现在 **Tools | Macros | <Your Package>**，以及任何包中的代码段显示在 **Tools | Snippets | <Your Package>**。

然而，正如开头所提到的，包有一些规则。 例如，在软件更新期间，``Package/User`` 永远不会被破坏。

.. sidebar:: ``User`` 包

  通常，未打包的资源存储在 ``Packages/User`` 中。如果你有一些松散的代码片段，宏或插件，这是一个保持它们的好地方。

.. _merging-and-order-of-precedence:

合并与优先顺序
-------------------------------

``Packages/Default`` 和 ``Packages/User`` 在合并文件时也会受到特殊处理（例如 ``.sublime-keymap`` 和 ``.sublime-settings`` 文件）。在合并之前，文件必须按顺序排列。为此，Sublime Text按名称按字母顺序对它们进行排序，但 ``Default`` 和 ``User`` 中包含的文件除外：``Default`` 将始终显示在列表的前面，而 ``User`` 将结束。


恢复包
******************

Sublime Text保留所有已安装软件包的副本，以便在需要时重新创建它们。这意味着它将能够重新安装核心软件包，随附的软件包和用户软件包。但是，只有作为 ``sublime-package`` 安装的用户软件包才会添加到已安装软件包的注册表中。如果删除它们，以其他方式安装的软件包将完全丢失。

将Sublime Text恢复为默认配置
---------------------------------------------------

要将Sublime Text恢复为其默认配置，请删除数据目录并重新启动编辑器。但请记住，``Installed Packages`` 目录也将被删除，因此您将丢失所有已安装的软件包。

在采取像这样的极端措施之前，务必确保备份数据。


``Installed Packages`` 目录
************************************

您将在数据目录中找到此目录。它包含每个安装的 ``sublime-package`` 的副本。用于恢复 ``Packages``。


``Pristine Packages`` 目录
***********************************

您将在数据目录中找到此目录。它包含每个运送和核心包的副本。用于恢复 ``Packages``。