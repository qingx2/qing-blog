---
title: Chrome cVim 快捷键速查表
date: 2017-08-24 21:26:55
---

![](http://7xoosr.com1.z0.glb.clouddn.com/%E7%99%BD%E5%87%A4.jpg)

<!--more-->

最近在各大论坛有意多关注了些Chrome推荐的插件，看见一款新玩意推荐 [cVim ](https://github.com/1995eaton/chromium-vim)说比Vimium还要强大，自带快捷键不说还集成了搜索、查找、viusal 模式、像素级平滑滚动、自定义键映射、更顺手标签页快捷键。

[GitHub地址](https://github.com/1995eaton/chromium-vim) https://github.com/1995eaton/chromium-vim

[Chrome Extension](https://chrome.google.com/webstore/detail/cvim/ihlenndgcmojhcghmfjfneahoeklbjjh) https://chrome.google.com/webstore/detail/cvim/ihlenndgcmojhcghmfjfneahoeklbjjh

根据官方的介绍，它和Vimium、ViChrome有几个明显的特征优势：

- 支持自定义搜索引擎
- 正则表达式
- 支持 Visual Mode
- 命令支持tab补全
- 自定义键映射



### 键映射

| 页面滚动                  |                                          | Mapping name                        |
| --------------------- | :--------------------------------------- | :---------------------------------- |
| `j`, `s`              | 向下滚动                                     | scrollDown                          |
| `k`, `w`              | 向上滑动                                     | scrollUp                            |
| `h`                   | 向左滚动                                     | scrollLeft                          |
| `l`                   | 向右滚动                                     | scrollRight                         |
| `d`                   | 滚动一半页面                                   | scrollPageDown                      |
| unmapped              | 滚动全页下来                                   | scrollFullPageDown                  |
| `u`, `e`              | 滚动一半页面                                   | scrollPageUp                        |
| unmapped              | 滚动全页面                                    | scrollFullPageUp                    |
| `gg`                  | 滚动到页面顶部                                  | scrollToTop                         |
| `G`                   | 滚动到页面的底部                                 | scrollToBottom                      |
| `0`                   | 滚动到页面的左边                                 | scrollToLeft                        |
| `$`                   | 滚动到页面的右侧                                 | scrollToRight                       |
| `#`                   | 将滚动焦点重置为主页面                              | resetScrollFocus                    |
| `gi`                  | 转到第一个输入框                                 | goToInput                           |
| `gI`                  | 转到最后一个聚焦的输入框 `gi`                        | goToLastInput                       |
| `zz`                  | 中心页到当前搜索匹配（中）                            | centerMatchH                        |
| `zt`                  | 中心页到当前搜索匹配（上）                            | centerMatchT                        |
| `zb`                  | 中心页面到当前搜索匹配（底部）                          | centerMatchB                        |
| **连接提示**              |                                          |                                     |
| `f`                   | 打开当前页面中的链接                               | createHint                          |
| `F`                   | 在新标签页中打开链接                               | createTabbedHint                    |
| unmapped              | 在新标签页中打开链接（活动）                           | createActiveTabbedHint              |
| `W`                   | 在新窗口中打开链接                                | createHintWindow                    |
| `A`                   | 重复最后提示命令                                 | openLastHint                        |
| `q`                   | 触发悬停事件                                   | createHoverHint                     |
| `Q`                   | 触发不起眼的事件                                 | createUnhoverHint                   |
| `mf`                  | 打开多个链接                                   | createMultiHint                     |
| unmapped              | 使用外部编辑器编辑文本                              | createEditHint                      |
| unmapped              | 调用带有链接的代码块作为第一个参数                        | createScriptHint(`<FUNCTION_NAME>`) |
| unmapped              | 在新标签页中打开图像                               | fullImageHint                       |
| `mr`                  | 反向图像搜索多个链接                               | multiReverseImage                   |
| `my`                  | 打开多个链接（打开链接P列表）                          | multiYankUrl                        |
| `gy`                  | 从链接复制URL到剪贴板                             | yankUrl                             |
| `gr`                  | 反向图像搜索（谷歌图像）                             | reverseImage                        |
| `;`                   | 改变链接提示的重点                                |                                     |
| **快速标记马克**            |                                          |                                     |
| `M<*>`                | 创建quickmark <*>                          | addQuickMark                        |
| `go<*>`               | 在当前标签中打开quickmark <*>                    | openQuickMark                       |
| `gn<*>`               | 在新标签页中打开quickmark <*>                    | openQuickMarkTabbed                 |
| `gw<*>`               | 在新窗口中打开quickmark <*>                     | openQuickMarkWindowed               |
| **杂项**                |                                          |                                     |
| `a`                   | 简写别名 ":tabnew google " (以google搜索打开新页面)  | :tabnew google                      |
| `.`                   | 重复最后一个命令                                 | repeatCommand                       |
| `:`                   | 打开命令栏                                    | openCommandBar                      |
| `/`                   | 打开搜索栏                                    | openSearchBar                       |
| `?`                   | 打开搜索栏（反向搜索）                              | openSearchBarReverse                |
| unmapped              | 打开链接搜索栏（与按下相同`/?`）                       | openLinkSearchBar                   |
| `I`                   | 搜索浏览器历史记录                                | :history                            |
| `<N>g%`               | 在页面上滚动<N>百分比                             | percentScroll                       |
| `<N>`unmapped         | 将`<N>`密钥传递到当前页面                          | passKeys                            |
| `zr`                  | 重新启动Google Chrome                        | :chrome://restart&lt;CR&gt;         |
| `i`                   | 进入插入模式（退出退出）                             | insertMode                          |
| `r`                   | 重新加载当前选项卡                                | reloadTab                           |
| `gR`                  | 重新加载当前选项卡+本地缓存                           | reloadTabUncached                   |
| `;<*>`                | 创建标记<*>                                  | setMark                             |
| `''`                  | 转到最后滚动位置                                 | lastScrollPosition                  |
| `<C-o>`               | 转到上一个滚动位置                                | previousScrollPosition              |
| `<C-i>`               | 转到下一个滚动位置                                | nextScrollPosition                  |
| `'<*>`                | 去mark的位置<*>                              | goToMark                            |
| `cm`                  | 静音/取消静音选项卡                               | muteTab                             |
| none                  | 重新加载所有选项卡                                | reloadAllTabs                       |
| `cr`                  | 刷新所有标签(不包括当前标签页)                         | reloadAllButCurrent                 |
| `zi`                  | 缩放页面                                     | zoomPageIn                          |
| `zo`                  | 缩小页面                                     | zoomPageOut                         |
| `z0`                  | 初始化页面大小                                  | zoomOrig                            |
| `z<Enter>`            | 切换图像缩放（与仅在图像页面上单击图像相同）                   | toggleImageZoom                     |
| `gd`                  | alias to :chrome://downloads&lt;CR&gt;   | :chrome://downloads&lt;CR&gt;       |
| `ge`                  | alias to :chrome://extensions&lt;CR&gt;  | :chrome://extensions&lt;CR&gt;      |
| `yy`                  | 将当前页面的URL复制到剪贴板                          | yankDocumentUrl                     |
| `yY`                  | 将当前帧的URL复制到剪贴板                           | yankRootUrl                         |
| `ya`                  | 复制当前窗口中的URL                              | yankWindowUrls                      |
| `yh`                  | 从查找模式复制当前匹配的文本（如果可用）                     | yankHighlight                       |
| `b`                   | 搜寻书签                                     | :bookmarks                          |
| `p`                   | 打开剪贴板选择                                  | openPaste                           |
| `P`                   | 在新标签页中打开剪贴板选择                            | openPasteTab                        |
| `gj`                  | 隐藏下载架                                    | hideDownloadsShelf                  |
| `gf`                  | cycle through iframes                    | nextFrame                           |
| `gF`                  | go to the root frame                     | rootFrame                           |
| `gq`                  | 停止当前选项卡加载                                | cancelWebRequest                    |
| `gQ`                  | 停止加载所有标签                                 | cancelAllWebRequests                |
| `gu`                  | 在URL中上传一条路径                              | goUpUrl                             |
| `gU`                  | 转到基本URL                                  | goToRootUrl                         |
| `gs`                  | 转到当前网址的view-source：//页面                  | :viewsource!                        |
| `<C-b>`               | 创建或切换当前网址的书签                             | createBookmark                      |
| unmapped              | 关闭所有浏览器窗口                                | quitChrome                          |
| `g-`                  | 减去URL路径中的第一个数字（例如`www.example.com/5`=> `www.example.com/4`） | decrementURLPath                    |
| `g+`                  | 增加URL路径中的第一个数字                           | incrementURLPath                    |
| **标签导航**              |                                          |                                     |
| `gt`, `K`, `R`        | 导航到下一个选项卡                                | nextTab                             |
| `gT`, `J`, `E`        | 导航到上一个选项卡                                | previousTab                         |
| `g0`, `g$`            | 转到第一个/最后一个选项卡                            | firstTab, lastTab                   |
| `<C-S-h>`, `gh`       | 在新标签页中打开当前标签的历史记录中的最后一个URL               | openLastLinkInTab                   |
| `<C-S-l>`, `gl`       | 在新标签页中打开当前选项卡的历史记录中的下一个URL               | openNextLinkInTab                   |
| `x`                   | 关闭当前选项卡                                  | closeTab                            |
| `gxT`                 | 关闭当前选项卡左侧的选项卡                            | closeTabLeft                        |
| `gxt`                 | 关闭当前选项卡右侧的选项卡                            | closeTabRight                       |
| `gx0`                 | 关闭当前选项卡左侧的所有选项卡                          | closeTabsToLeft                     |
| `gx$`                 | 关闭当前选项卡右侧的所有选项卡                          | closeTabsToRight                    |
| `X`                   | 打开最后关闭的标签页                               | lastClosedTab                       |
| `t`                   | :tabnew                                  | :tabnew                             |
| `T`                   | :tabnew &lt;CURRENT URL&gt;              | :tabnew @%                          |
| `O`                   | :open &lt;CURRENT URL&gt;                | :open @%                            |
| `<N>%`                | 切换到选项卡<N>                                | goToTab                             |
| `H`, `S`              | 回去                                       | goBack                              |
| `L`, `D`              | 前进                                       | goForward                           |
| `B`                   | 搜索另一个活动选项卡                               | :buffer                             |
| `<`                   | 向左移动当前标签                                 | moveTabLeft                         |
| `>`                   | 向右移动当前标签                                 | moveTabRight                        |
| `]]`                  | 点击页面上的“下一个”链接（见上面的nextmatchpattern）      | nextMatchPattern                    |
| `[[`                  | 点击页面上的“返回”链接（参见上面的上一个模式）                 | previousMatchPattern                |
| `gp`                  | 引导/取消固定当前选项卡                             | pinTab                              |
| `<C-6>`               | 在最后使用的标签之间切换焦点                           | lastUsedTab                         |
| 查找模式                  |                                          |                                     |
| `n`                   | 下一个搜索结果                                  | nextSearchResult                    |
| `N`                   | 以前的搜索结果                                  | previousSearchResult                |
| `v`                   | 进入视觉/插入模式（突出显示当前搜索/选择）                   | toggleVisualMode                    |
| `V`                   | 从插入模式/当前突出显示的搜索进入视线模式                    | toggleVisualLineMode                |
| unmapped              | 清除搜索模式突出显示                               | clearSearchHighlight                |
| **Visual/Caret Mode** |                                          |                                     |
| `<Esc>`               | 将视觉模式退出插入模式/退出插入模式到正常模式                  |                                     |
| `v`                   | 在视觉/插入模式之间切换                             |                                     |
| `h`, `j`, `k`, `l`    | 移动插入位置/扩展视觉选择                            |                                     |
| `y`                   | 配合当前的选择                                  |                                     |
| `n`                   | 选择下一个搜索结果                                |                                     |
| `N`                   | 选择先前的搜索结果                                |                                     |
| `p`                   | 打开当前选项卡中的突出显示                            |                                     |
| `P`                   | 在新标签中打开突出显示的文本                           |                                     |
| **文本框**               |                                          |                                     |
| `<C-i>`               | 将光标移动到行的开头                               | beginningOfLine                     |
| `<C-e>`               | 将光标移动到行尾                                 | endOfLine                           |
| `<C-u>`               | 删除到行的开头                                  | deleteToBeginning                   |
| `<C-o>`               | 删除到行尾                                    | deleteToEnd                         |
| `<C-y>`               | 向后删除一个字                                  | deleteWord                          |
| `<C-p>`               | 向前删除一个字                                  | deleteForwardWord                   |
| unmapped              | 向后删除一个字符                                 | deleteChar                          |
| unmapped              | 向前删除一个字符                                 | deleteForwardChar                   |
| `<C-h>`               | 将光标移回一个字                                 | backwardWord                        |
| `<C-l>`               | 将光标向前移动一个字                               | forwardWord                         |
| `<C-f>`               | 将光标向前移动一个字母                              | forwardChar                         |
| `<C-b>`               | 将光标移回一个字母                                | backwardChar                        |
| `<C-j>`               | 将光标向前移动一行                                | forwardLine                         |
| `<C-k>`               | 将光标移回一行                                  | backwardLine                        |
| unmapped              | 全选输入文本（相当于`<C-a>`）                       | selectAll                           |
| unmapped              | 在终端中使用Vim进行编辑（需要运行该工作的[cvim_server.py](https://github.com/1995eaton/chromium-vim/blob/master/cvim_server.py)脚本以及该脚本中的VIM_COMMAND集） | editWithVim                         |



### 命令行模式

| Command                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- |
| :tabnew (autocomplete)                   | 打开一个新的标签与打字/完成的搜索                        |
| :new (autocomplete)                      | 用打字/完成搜索打开一个新窗口                          |
| :open (autocomplete)                     | 打开输入/完成的URL / Google搜索                   |
| :history (autocomplete)                  | 搜索浏览器历史记录                                |
| :bookmarks (autocomplete)                | 搜寻书签                                     |
| :bookmarks /&lt;folder&gt; (autocomplete) | 按文件夹浏览书签/从文件夹中打开所有书签                     |
| :set (autocomplete)                      | 暂时更改cVim设置                               |
| :chrome:// (autocomplete)                | 打开一个 chrome：// URL                       |
| :tabhistory (autocomplete)               | 浏览当前选项卡的不同历史状态                           |
| :command `<NAME>` `<ACTION>`             | aliases :`<NAME>` to :`<ACTION>`         |
| :quit                                    | 关闭当前选项卡                                  |
| :qall                                    | 关闭当前窗口                                   |
| :restore (autocomplete)                  | 恢复以前关闭的标签（仅限较新版本的Chrome）                 |
| :tabattach (autocomplete)                | 将当前选项卡移动到另一个打开的窗口                        |
| :tabdetach                               | 将当前选项卡移动到新窗口                             |
| :file (autocomplete)                     | 打开本地文件                                   |
| :source (autocomplete)                   | 将cVimrc文件加载到内存中（如果`localconfig`以前设置了该设置，这将覆盖选项页面中的设置 |
| :duplicate                               | 复制当前选项卡                                  |
| :settings                                | 打开设置页面                                   |
| :nohlsearch                              | 清除上次搜索中突出显示的文字                           |
| :execute                                 | 执行一个键序列（对映射有用，例如“map j：execute 2j”）      |
| :buffer (autocomplete)                   | 更改为不同的选项卡                                |
| :mksession                               | 从活动窗口中的当前选项卡创建一个新的会话                     |
| :delsession (autocomplete)               | 删除已保存的会话                                 |
| :session (autocomplete)                  | 在新窗口中从保存的会话中打开选项卡                        |
| :script                                  | 在当前页面上运行JavaScript                       |
| :togglepin                               | 切换当前选项卡的引脚状态                             |
| :pintab                                  | 固定当前选项卡                                  |
| :unpintab                                | 取消固定当前选项卡                                |

#
