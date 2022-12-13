# Latex+VSCode设置

VSCode支持Latex文档编写，页面漂亮、并提供单词、语法检测。

## 1. 安装texlive

从相关开源镜像网站下载texlive镜像文件，具体步骤略。

## 2. 安装VSCode插件

扩展商店中搜索`Latex Workshop`，安装。

## 3. 配置VSCode的Latex插件

这一步骤主要设置latex的编译工具、命令以及编译链。

1. 打开设置，点击右上角第二个图标`打开设置(json)`，进入`..../Code/User/{}settings.json`编辑。
2. 将下面的配置直接拷贝：
```json
{
    "latex-workshop.latex.autoBuild.run": "never",
    "latex-workshop.showContextMenu": true,
    "latex-workshop.intellisense.package.enabled": true,
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
   "latex-workshop.latex.recipes": [
        {
            "name": "pdfLaTeX",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "pdflatex ➞ bibtex ➞ pdflatex × 2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        }
    ],
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click"
}
```

## 4. 配置解读
- `"latex-workshop.latex.autoBuild.run": "never"`
   
    设置何时使用默认的(第一个)编译链自动构建LaTeX项目，即什么时候自动进行代码的编译。有三个选项：
   1. **onFileChange**: 在检测任何依赖项中的文件更改(甚至被其他应用程序修改)时构建项目，即当检测到代码被更改时就自动编译tex文件；
   2. **onSave**: 当代码被保存时自动编译文件；
   3. **never**: 从不自动编译，即需编写者手动编译文档


- `"latex-workshop.showContextMenu": true`
    
    启用上下文LaTeX菜单。此菜单默认状态下停用，即变量设置为`false`，因为它可以通过新的LaTeX标记使用（新的LaTeX标记能够编译文档，将在下文提及）。只需将此变量设置为`true`即可恢复菜单。即此命令设置是否将编译文档的选项出现在鼠标右键的菜单中。

- `"latex-workshop.message.error.show"  : false,`
- `"latex-workshop.message.warning.show": false`
    
    这两个命令是设置当文档编译错误时是否弹出显示出错和警告的弹窗。

- `"latex-workshop.latex.tools"`

    这些代码是定义在下文`recipes`编译链中被使用的编译命令，此处为默认配置，不需要进行更改。其中的`name`为这些命令的标签，用作下文`recipes`的引用；而`command`为在该拓展中的编译方式。


- `"latex-workshop.latex.recipes"`
  
    此串代码是对编译链进行定义，其中`name`是标签，也就是出现在工具栏中的链名称；`tool`是`name`标签所对应的编译顺序，其内部编译命令来自上面recipes中内容。
    
    可以看到的是，在编译链中定义的命令出现在了vscode右侧的工具栏中。

- `"latex-workshop.latex.clean.fileTypes"`
  
    这串命令则是设置编译完成后要清除掉的辅助文件类型，若无特殊需求，无需进行更改。

- `"latex-workshop.latex.autoClean.run": "onFailed"`
  
  这条命令是设置什么时候对上文设置的辅助文件进行清除。其变量有：
  1. **onBuilt**: 无论是否编译成功，都选择清除辅助文件；
  2. **onFailed**: 当编译失败时，清除辅助文件；
  3. **never**: 无论何时，都不清除辅助文件。

- `"latex-workshop.latex.recipe.default": "lastUsed"`
  
  该命令的作用为设置vscode编译 tex 文档时的默认编译链。有两个变量:
  1. **first**: 使用latex-workshop.latex.recipes中的第一条编译链，故而您可以根据自己的需要更改编译链顺序； 
  2. **lastUsed**: 使用最近一次编译所用的编译链。

- `"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click"`
  
  用于反向同步（即从编译出的pdf文件指定位置跳转到tex文件中相应代码所在位置）的内部查看器的快捷键绑定。变量有两种：
  1. **ctrl-click** ： 为默认选项，使用Ctrl/cmd+鼠标左键单击；
  2. **double-click** : 使用鼠标左键双击。


## 5. 常见问题

### 1. **PDFLaTeX** 编译模式与 **XeLaTeX** 的区别
> 1. PDFLaTeX 使用的是TeX的标准字体，所以生成PDF时，会将所有的非 TeX 标准字体进行替换，其生成的 PDF 文件默认嵌入所有字体；而使用 XeLaTeX 编译，如果说论文中有很多图片或者其他元素没有嵌入字体的话，生成的 PDF 文件也会有些字体没有嵌入。
> 2. XeLaTeX 对应的 XeTeX 对字体的支持更好，允许用户使用操作系统字体来代替 TeX 的标准字体，而且对非拉丁字体的支持更好。
> 3. PDFLaTeX 进行编译的速度比 XeLaTeX 速度快。

### 2. 编译链的作用
> 编译链的存在是为了更方便编译，因为如果涉及到.bib文件，就需要进行多次不同命令的转换编译，比较麻烦，而编译链就解决了这个问题。

## 6. 参考

1. [Visual Studio Code (vscode)配置LaTeX](https://zhuanlan.zhihu.com/p/166523064)
2. [PDFLaTeX和XeLaTeX有什么区别](https://blog.csdn.net/qysh123/article/details/11833649?utm_source=tuicool)
3. [使用VSCode编写LaTeX](https://zhuanlan.zhihu.com/p/38178015)