# [TeXt Theme](https://github.com/kitian616/jekyll-TeXt-theme)

[![Gem Version](https://img.shields.io/gem/v/jekyll-text-theme.svg)](https://github.com/kitian616/jekyll-TeXt-theme/releases)
[![license](https://img.shields.io/github/license/kitian616/jekyll-TeXt-theme.svg)](https://github.com/kitian616/jekyll-TeXt-theme/blob/master/LICENSE)
[![Travis](https://img.shields.io/travis/kitian616/jekyll-TeXt-theme.svg)](https://travis-ci.org/kitian616/jekyll-TeXt-theme)

![TeXt Theme](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/TeXt-home.png)

![TeXt Theme Details](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/TeXt-details.png)

[Demo](https://tianqi.name/jekyll-TeXt-theme/) | [English](https://github.com/kitian616/jekyll-TeXt-theme/blob/master/README-en.md)

TeXt is a succinct theme for blogging.

TeXt 是針對博客的一款簡潔的主題，它雖然簡潔但並不簡單。它參考了 iOS 11 的風格，有大而突出的標題和圓潤的按鈕及卡片。

## Features

- 回應式
- 分頁（[jekyll-paginate](https://github.com/jekyll/jekyll-paginate)）
- 文章目錄
- 文章標籤
- 搜索（標題）
- 閱讀次數統計（[LeanCloud](https://leancloud.cn/)）
- Emoji（[Jemoji](https://github.com/jekyll/jemoji)）
- 評論（[Disqus](https://disqus.com/), [gitalk](https://gitalk.github.io/)）
- Google Analytics
- 聯繫方式設置（Email, Facebook, Twitter, 微博, 知乎……）
- Web 語意化
- 網站圖示的自動化工具（[gulp-svg2png](https://www.npmjs.com/package/gulp-svg2png), [gulp-to-ico](https://www.npmjs.com/package/gulp-to-ico)）
- Color Theme
- 數學公式（[MathJax](https://www.mathjax.org/)）
- 流程圖， 序列圖，甘特圖（[mermaid](https://mermaidjs.github.io/)）
- 柱狀圖，折線圖，圓形圖，雷達圖（[chartjs](http://www.chartjs.org/)）
- RSS（[jekyll-feed](https://github.com/jekyll/jekyll-feed)）
- 多語言支援（English | 簡體中文 | 繁體中文）

下面簡要的介紹下使用的方法，當然如果你對 Jekyll 比較瞭解可以直接看後面的高級部分，這是該主題增加的一些特有功能。

## How To Use

最簡單的方法是直接 **Fork** 到你的 GitHub 倉庫然後更改其名稱為 `<username>.github.io`，稍等一會兒訪問 `https://<username>.github.io` 即可看到一個空的博客頁，接下來你可以把它 Clone 到本地修改後提交。

當然你也可以在 [Releases 頁面](https://github.com/kitian616/jekyll-TeXt-theme/releases) 下載最新版本源碼，或直接 Clone 代碼到本地。

另外，因為每個版本都是作為一個 [Gem](https://rubygems.org/gems/jekyll-text-theme) 發佈的，所以你也可以通過 Jekyll 的主題系統安裝該主題，這種方式可以很方便的升級保持最新，但不支援 GitHub 的自動編譯，詳見 [Jekyll: 主題](http://jekyllcn.com/docs/themes/)。項目的 ./test 目錄就是一個使用主題系統的例子。

### 配置

在 ./\_config.yml 檔裡按照說明加上你的資訊，例如你的名字和聯繫方式，網站的標題和描述等等。

在 ./about.md 中寫上你的簡單介紹，例如我叫小明之類的。

### 寫博客

使用 Markdown 編寫文章，位於 ./\_posts 目錄（需要自行創建）下，檔案名採用日期 + 標題的形式，形如 `2017-02-02-Very-Long-Title`，可參考 ./test/\_posts 目錄。

可以在頭資訊裡設置文章的一些基本資訊，包括標題、發佈時間和標籤等。當然，如果你不設置標題和發佈時間，系統會使用檔案名中的標題和發佈時間，詳見 [Jekyll: 頭信息](http://jekyllcn.com/docs/frontmatter/)。當然，該主題在原有的基礎上增加了一些屬性，這在後面會講到。

#### 摘要

該主題的摘要有兩種模式——TEXT 模式和 HTML 模式。 當 ./\_config.yml 配置項 `excerpt_type` 的值為 `text` 時是 TEXT 模式，為 `html` 時是 HTML 模式，**預設為 TEXT 模式**。

TEXT 模式的摘要為純文字，會過濾掉一切非文本元素（標題，連結，清單，表格，圖片等等），且截取前 350 個字元。

HTML 模式的摘要為 HTML 文檔，與文章內容一致，並且 **預設展示整篇文章的內容**。若想控制摘要內容，需要在文章中想要顯示到的地方加上 `<!--more-->`，詳見 [Jekyll: 文章摘要](http://jekyll.com.cn/docs/posts/#_6)。

> 提示：為了首頁更好的展示效果，個人還是推薦使用 HTML 模式，並自己在文章中加上 `<!--more-->`。

### 安裝環境（非必須）

具體可參考 [Jekyll: 安裝](http://jekyllcn.com/docs/installation/)。

請確保你的電腦上配置好了 Ruby 開發環境。(ruby, bundle, Command Line Tools(macOS) ...)

首先安裝 github-pages（包含了 Jekyll 以及一些外掛程式），在專案根目錄執行 `bundle install` 即可安裝。

推薦安裝 Node.js 環境，可以獲得更好的開發體驗。

### 本機服務（非必須）

如果你安裝了 Node.js 環境，只需要在專案根目錄運行 `npm run dev` 即可啟動本機服務。

如果沒有安裝 Node.js 環境，則是：

```console
bundle exec jekyll serve -H 0.0.0.0
```

命令執行成功後在流覽器中訪問 [http://localhost:4000/](http://localhost:4000/) 即可看到頁面。

### 部署與提交

推薦部署到 GitHub Pages 上，簡單而免費，詳見 [Jekyll: GitHub Pages](http://jekyllcn.com/docs/github-pages/)。

如果你是下載或者 Clone 的源碼，那麼你需要在 GitHub 上建立一個 Repository，然後把專案代碼 push 到其對應的分支上（如果以 `<username>.github.io` 命名則對應分支為 `master` ，其他的為 `gh-pages`，詳見 [Github Pages: Configuring a publishing source for GitHub Pages](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)）。

如果你是通過 Jekyll 的主題系統安裝的，那麼你需要把本地編譯好的代碼 push 到上文所說的對應分支上。

當然你也可以部署到到其他地方。

## 高級

### 多語言

該主題支援 English、簡體中文和繁體中文，只需在 ./\_config.yml 中設置對應 `lang` 項即可。設置後整個網站的主題文字（導航，閱讀更多，文章數統計，日期格式，文章協議等等）會變為設置的語言，多語言的設定檔為 ./_data/locale.yml，你可以自由的修改和增加語言。

另外，該主題也支援對某篇文章（頁面）單獨設置語言，只需在 Markdown 或頁面 HTML 檔的頭資訊中設置 `lang` 項，其優先順序高於 ./\_config.yml 中設置的值。設置後該文章（頁面）的主題文字會變為頭資訊中設置的語言。

> 提示：當前的 `lang` 值可選值為 en(English), zh(簡體中文), zh-Hans(簡體中文), zh-Hant(繁體中文)。

### Color Theme

顏色主題位於資料夾 ./\_sass/colors 中，修改 ./\_config.yml 中的 text_color_theme 項為以下值即可更換顏色主題，預設主題為 default。

| `default` | `dark` | `forest` |
| --- |  --- | --- |
| ![default](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/colors_default.png) | ![dark](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/colors_dark.png) | ![forest](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/colors_forest.png) |

| `ocean` | `chocolate` | `orange` |
| --- |  --- | --- |
| ![ocean](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/colors_ocean.png) | ![chocolate](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/colors_chocolate.png) | ![orange](https://raw.githubusercontent.com/kitian616/jekyll-TeXt-theme/master/screenshots/colors_orange.png) |

更多顏色主題敬請期待。

### 網站圖示

該主題自帶了一個“銀杏葉”圖示，你可以把它替換為自己的圖示。網站的圖示位於 ./favicon.ico 和 ./assets/images/logo 目錄下。你會看到 logo 目錄中有很多的 png 檔和一個 svg 向量圖檔。那些 png 圖片實際上就是根據 svg 向量圖生成的不同大小的圖片，這些圖片是一些場景可能會用到的大圖示，像 iOS 和 Android 的固定到螢幕和 Windows 10 的磁貼。

該主題提供了一個自動化腳本能將 svg 向量圖自動生成 favicon 和 png 檔。你所要做的是：

1. 安裝 Node.js 環境

2. 在專案根目錄執行 `npm i` 命令

3. 替換 ./assets/images/logo 目錄下的 logo.svg 文件

4. 執行 `npm run artwork` 命令，此時 favicon 和 png 便會替換為新 logo.svg 生成的檔

當然如果要追求各個尺寸下圖示的顯示效果，那還得對不同尺寸的圖片進行修改和優化。

### 評論系統

目前支援 Disqus 和 gitalk 評論系統，優先使用 Disqus。

#### Disqus

在 ./\_config.yml 文件的 `disqus.shortname` 項填上你在 [Disqus](https://disqus.com/) 上為網站建立的 site 對應的 shortname 即可，需要注意的是 Disqus 在大陸是無法直接訪問的。

#### gitalk

在 ./\_config.yml 文件的 `gitalk` 的子項（`clientID`,`clientSecret`, `repository`, `owner`, `admin`）填上 gitalk 的對應參數 即可，詳見 [gitalk 中文文檔](https://github.com/gitalk/gitalk/blob/master/readme-cn.md)。

> 注意：使用評論系統必須在文章的頭資訊中設置 key 值（可用字元集：`字母`、`數位` 及 `- _ : .`）。

### 閱讀量統計

在 ./\_config.yml 文件 `leancloud` 的 `app_id`、`app_key`、`app_class` 項分別填上你在 [LeanCloud](https://leancloud.cn) 為網站建立的應用的對應參數。

> 注意：使用閱讀量統計必須在文章的頭資訊中設置 key 值（可用字元集：`字母`、`數位` 及 `- _ : .`）。

### Google Analytics

在 ./\_config.yml 文件的 `ga_tracking_id` 項填上你在 [Google Analytics](https://analytics.google.com) 上為網站建立的媒體資源對應的跟蹤 ID。

### Markdown 頭資訊增強

除了 Jekyll 官方的頭資訊外，該主題增加了一些頭資訊。

| 變數名稱       | 可選值          | 描述 |
| ---           | ---           | --- |
| key           |               | 評論系統和閱讀量統計使用的文章識別字，如果未設置則評論和統計無效。可用字元集：`字母`、`數位` 及 `- _ : .` |
| lang          | en/zh/zh-Hans/zh-Hant | 該文章的語言，其優先順序高於  ./\_config.yml 中設置的值 |
| modify_date   |               | 該文章的修改時間，不影響首頁文章排序（`date` 代表發表時間，會影響文章排序） |
| comment       | true/false    | 該文章是否能夠評論，默認為 true（當然你也可以通過不設置 key 來實現，但是這樣的話統計也失效了） |
| mathjax       | true/false    | 該文章是否需要使用 MathJax 公式，預設為 false（此時只會在該文章頁面中解析 MathJax 公式。當然你也可以配置 _config.yml 中的 `mathjax` 項為 true，讓網站全域支持 MathJax 公式） |
| mermaid       | true/false    | 該文章是否需要使用 Mermaid 繪製流程圖 |
| chart         | true/false    | 該文章是否需要使用 Chart 繪製圖示 |

### 其他資源

在 ./\_includes/icon/social 目錄下有很多的社交產品圖示，例如 Behance、Flickr、QQ、微信等，方便修改和使用。

## 示例

- [Demo](https://tianqi.name/jekyll-TeXt-theme/)
- [Qi's blog](https://tianqi.name/blog/)
