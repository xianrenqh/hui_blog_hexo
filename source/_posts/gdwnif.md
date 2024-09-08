---
title: 快速将Butterfly主题的CDN从jsDelivr切换至自建反向代理源
urlname: gdwnif
date: '2022-06-01 07:22:01 +0000'
tags: []
categories: []
---

tags: [cdn,jsDeliver,butterfly]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.net

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">最新版的Butterfly取消了原来设置在</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">_config.yml</font><font style="color:rgb(51, 51, 51);">里的默认CDN，导致不能快速替换掉现在极不稳定的jsDelivr CDN。本文的默认Butterfly版本为</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">4.1.0</font><font style="color:rgb(51, 51, 51);">。</font>

# <font style="color:rgb(0, 0, 0);">配置</font>

<font style="color:rgb(51, 51, 51);">现在的默认 CDN 地址被放在了主题的</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">/scripts/events/config.js</font><font style="color:rgb(51, 51, 51);">中：</font>

```javascript
/**
 * Butterfly
 * 1. Merge CDN
 * 2. Capitalize the first letter of comment name
 */

"use strict";

hexo.extend.filter.register("before_generate", () => {
  const themeConfig = hexo.theme.config;

  /**
   * Merge CDN
   */

  const defaultCDN = {
    main_css: "/css/index.css",
    main: "/js/main.js",
    utils: "/js/utils.js",

    // pjax
    pjax: "https://cdn.jsdelivr.net/npm/pjax/pjax.min.js",

    // comments
    gitalk: "https://cdn.jsdelivr.net/npm/gitalk@latest/dist/gitalk.min.js",
    gitalk_css: "https://cdn.jsdelivr.net/npm/gitalk/dist/gitalk.min.css",
    blueimp_md5: "https://cdn.jsdelivr.net/npm/blueimp-md5/js/md5.min.js",
    valine: "https://cdn.jsdelivr.net/npm/valine/dist/Valine.min.js",
    disqusjs: "https://cdn.jsdelivr.net/npm/disqusjs@1/dist/disqus.js",
    disqusjs_css: "https://cdn.jsdelivr.net/npm/disqusjs@1/dist/disqusjs.css",
    utterances: "https://utteranc.es/client.js",
    twikoo: "https://cdn.jsdelivr.net/npm/twikoo/dist/twikoo.all.min.js",
    waline: "https://cdn.jsdelivr.net/npm/@waline/client/dist/Waline.min.js",
    giscus: "https://giscus.app/client.js",

    // share
    addtoany: "https://static.addtoany.com/menu/page.js",
    sharejs:
      "https://cdn.jsdelivr.net/npm/social-share.js/dist/js/social-share.min.js",
    sharejs_css:
      "https://cdn.jsdelivr.net/npm/social-share.js/dist/css/share.min.css",

    // search
    local_search: "/js/search/local-search.js",
    algolia_js: "/js/search/algolia.js",
    algolia_search_v4:
      "https://cdn.jsdelivr.net/npm/algoliasearch@4/dist/algoliasearch-lite.umd.js",
    instantsearch_v4:
      "https://cdn.jsdelivr.net/npm/instantsearch.js@4/dist/instantsearch.production.min.js",

    // math
    mathjax: "https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js",
    katex: "https://cdn.jsdelivr.net/npm/katex@latest/dist/katex.min.css",
    katex_copytex:
      "https://cdn.jsdelivr.net/npm/katex@latest/dist/contrib/copy-tex.min.js",
    katex_copytex_css:
      "https://cdn.jsdelivr.net/npm/katex@latest/dist/contrib/copy-tex.css",
    mermaid: "https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js",

    // count
    busuanzi: "//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js",

    // background effect
    canvas_ribbon:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/canvas-ribbon.min.js",
    canvas_fluttering_ribbon:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/canvas-fluttering-ribbon.min.js",
    canvas_nest:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/canvas-nest.min.js",

    lazyload:
      "https://cdn.jsdelivr.net/npm/vanilla-lazyload/dist/lazyload.iife.min.js",
    instantpage: "https://cdn.jsdelivr.net/npm/instant.page/instantpage.min.js",
    typed: "https://cdn.jsdelivr.net/npm/typed.js/lib/typed.min.js",
    pangu: "https://cdn.jsdelivr.net/npm/pangu/dist/browser/pangu.min.js",

    // photo
    fancybox_css_v4:
      "https://cdn.jsdelivr.net/npm/@fancyapps/ui/dist/fancybox.css",
    fancybox_v4:
      "https://cdn.jsdelivr.net/npm/@fancyapps/ui/dist/fancybox.umd.js",
    medium_zoom:
      "https://cdn.jsdelivr.net/npm/medium-zoom/dist/medium-zoom.min.js",

    // snackbar
    snackbar_css:
      "https://cdn.jsdelivr.net/npm/node-snackbar/dist/snackbar.min.css",
    snackbar: "https://cdn.jsdelivr.net/npm/node-snackbar/dist/snackbar.min.js",

    // effect
    activate_power_mode:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/activate-power-mode.min.js",
    fireworks:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/fireworks.min.js",
    click_heart:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/click-heart.min.js",
    ClickShowText:
      "https://cdn.jsdelivr.net/npm/butterfly-extsrc@1/dist/click-show-text.min.js",

    // fontawesome
    fontawesomeV6:
      "https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6/css/all.min.css",

    // Conversion between Traditional and Simplified Chinese
    translate: "/js/tw_cn.js",

    // flickr-justified-gallery
    flickr_justified_gallery_js:
      "https://cdn.jsdelivr.net/npm/flickr-justified-gallery@2/dist/fjGallery.min.js",
    flickr_justified_gallery_css:
      "https://cdn.jsdelivr.net/npm/flickr-justified-gallery@2/dist/fjGallery.min.css",

    // aplayer
    aplayer_css: "https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css",
    aplayer_js: "https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js",
    meting_js:
      "https://cdn.jsdelivr.net/gh/metowolf/MetingJS@1.2/dist/Meting.min.js",

    // Prism.js
    prismjs_js: "https://cdn.jsdelivr.net/npm/prismjs/prism.min.js",
    prismjs_lineNumber_js:
      "https://cdn.jsdelivr.net/npm/prismjs/plugins/line-numbers/prism-line-numbers.min.js",
    prismjs_autoloader:
      "https://cdn.jsdelivr.net/npm/prismjs/plugins/autoloader/prism-autoloader.min.js",
  };

  // delete null value
  const deleteNullValue = (obj) => {
    for (const i in obj) {
      obj[i] === null && delete obj[i];
    }
    return obj;
  };

  themeConfig.CDN = Object.assign(defaultCDN, deleteNullValue(themeConfig.CDN));

  /**
   * Capitalize the first letter of comment name
   */

  let { use } = themeConfig.comments;

  if (!use) return;

  if (typeof use === "string") {
    use = use.split(",");
  }

  const newArray = use.map((item) =>
    item.toLowerCase().replace(/\b[a-z]/g, (s) => s.toUpperCase())
  );

  themeConfig.comments.use = newArray;
});
```

<font style="color:rgb(51, 51, 51);">所以只要快速替换掉这里的 CDN，就可以切换到我们自建的 CDN 上。</font>

# <font style="color:rgb(0, 0, 0);">成果</font>

<font style="color:rgb(51, 51, 51);">将</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">cdn.jsdelivr.net</font><font style="color:rgb(51, 51, 51);">全部替换为自己的反代源后，成果就诞生了：</font>

**<font style="color:#F759AB;">（以下 2 种任选一种即可）</font>**

## <font style="color:rgb(51, 51, 51);">【代理后的 1 成果】</font>

```javascript
/**
 * Butterfly
 * 1. Merge CDN
 * 2. Capitalize the first letter of comment name
 */

"use strict";

hexo.extend.filter.register("before_generate", () => {
  const themeConfig = hexo.theme.config;

  /**
   * Merge CDN
   */

  const defaultCDN = {
    main_css: "/css/index.css",
    main: "/js/main.js",
    utils: "/js/utils.js",

    // pjax
    pjax: "https://unpkg.com/pjax/pjax.min.js",

    // comments
    gitalk: "https://unpkg.com/gitalk@latest/dist/gitalk.min.js",
    gitalk_css: "https://unpkg.com/gitalk/dist/gitalk.min.css",
    blueimp_md5: "https://unpkg.com/blueimp-md5/js/md5.min.js",
    valine: "https://unpkg.com/valine/dist/Valine.min.js",
    disqusjs: "https://unpkg.com/disqusjs@1/dist/disqus.js",
    disqusjs_css: "https://unpkg.com/disqusjs@1/dist/disqusjs.css",
    utterances: "https://utteranc.es/client.js",
    twikoo: "https://unpkg.com/twikoo/dist/twikoo.all.min.js",
    waline: "https://unpkg.com/@waline/client/dist/Waline.min.js",
    giscus: "https://giscus.app/client.js",

    // share
    addtoany: "https://static.addtoany.com/menu/page.js",
    sharejs: "https://unpkg.com/social-share.js/dist/js/social-share.min.js",
    sharejs_css: "https://unpkg.com/social-share.js/dist/css/share.min.css",

    // search
    local_search: "/js/search/local-search.js",
    algolia_js: "/js/search/algolia.js",
    algolia_search_v4:
      "https://unpkg.com/algoliasearch@4/dist/algoliasearch-lite.umd.js",
    instantsearch_v4:
      "https://unpkg.com/instantsearch.js@4/dist/instantsearch.production.min.js",

    // math
    mathjax: "https://unpkg.com/mathjax@3/es5/tex-mml-chtml.js",
    katex: "https://unpkg.com/katex@latest/dist/katex.min.css",
    katex_copytex:
      "https://unpkg.com/katex@latest/dist/contrib/copy-tex.min.js",
    katex_copytex_css:
      "https://unpkg.com/katex@latest/dist/contrib/copy-tex.css",
    mermaid: "https://unpkg.com/mermaid/dist/mermaid.min.js",

    // count
    busuanzi: "//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js",

    // background effect
    canvas_ribbon:
      "https://unpkg.com/butterfly-extsrc@1/dist/canvas-ribbon.min.js",
    canvas_fluttering_ribbon:
      "https://unpkg.com/butterfly-extsrc@1/dist/canvas-fluttering-ribbon.min.js",
    canvas_nest: "https://unpkg.com/butterfly-extsrc@1/dist/canvas-nest.min.js",

    lazyload:
      "https://jsdelivr.pai233.top/npm/vanilla-lazyload/dist/lazyload.iife.min.js",
    instantpage:
      "https://jsdelivr.pai233.top/npm/instant.page/instantpage.min.js",
    typed: "https://unpkg.com/typed.js/lib/typed.min.js",
    pangu: "https://unpkg.com/pangu/dist/browser/pangu.min.js",

    // photo
    fancybox_css_v4: "https://unpkg.com/@fancyapps/ui/dist/fancybox.css",
    fancybox_v4: "https://unpkg.com/@fancyapps/ui/dist/fancybox.umd.js",
    medium_zoom: "https://unpkg.com/medium-zoom/dist/medium-zoom.min.js",

    // snackbar
    snackbar_css: "https://unpkg.com/node-snackbar/dist/snackbar.min.css",
    snackbar: "https://unpkg.com/node-snackbar/dist/snackbar.min.js",

    // effect
    activate_power_mode:
      "https://unpkg.com/butterfly-extsrc@1/dist/activate-power-mode.min.js",
    fireworks: "https://unpkg.com/butterfly-extsrc@1/dist/fireworks.min.js",
    click_heart: "https://unpkg.com/butterfly-extsrc@1/dist/click-heart.min.js",
    ClickShowText:
      "https://unpkg.com/butterfly-extsrc@1/dist/click-show-text.min.js",

    // fontawesome
    fontawesomeV6:
      "https://unpkg.com/@fortawesome/fontawesome-free@6/css/all.min.css",

    // Conversion between Traditional and Simplified Chinese
    translate: "/js/tw_cn.js",

    // flickr-justified-gallery
    flickr_justified_gallery_js:
      "https://unpkg.com/flickr-justified-gallery@2/dist/fjGallery.min.js",
    flickr_justified_gallery_css:
      "https://unpkg.com/flickr-justified-gallery@2/dist/fjGallery.min.css",

    // aplayer
    aplayer_css: "https://unpkg.com/aplayer/dist/APlayer.min.css",
    aplayer_js: "https://unpkg.com/aplayer/dist/APlayer.min.js",
    meting_js:
      "https://cdn.jsdelivr.net/gh/metowolf/MetingJS@1.2/dist/Meting.min.js",

    // Prism.js
    prismjs_js: "https://unpkg.com/prismjs/prism.min.js",
    prismjs_lineNumber_js:
      "https://unpkg.com/prismjs/plugins/line-numbers/prism-line-numbers.min.js",
    prismjs_autoloader:
      "https://unpkg.com/prismjs/plugins/autoloader/prism-autoloader.min.js",
  };

  // delete null value
  const deleteNullValue = (obj) => {
    for (const i in obj) {
      obj[i] === null && delete obj[i];
    }
    return obj;
  };

  themeConfig.CDN = Object.assign(defaultCDN, deleteNullValue(themeConfig.CDN));

  /**
   * Capitalize the first letter of comment name
   */

  let { use } = themeConfig.comments;

  if (!use) return;

  if (typeof use === "string") {
    use = use.split(",");
  }

  const newArray = use.map((item) =>
    item.toLowerCase().replace(/\b[a-z]/g, (s) => s.toUpperCase())
  );

  themeConfig.comments.use = newArray;
});
```

## <font style="color:rgb(51, 51, 51);">【代理后的 2 成果】</font>

```javascript
/**
 * Butterfly
 * 1. Merge CDN
 * 2. Capitalize the first letter of comment name
 */

"use strict";

hexo.extend.filter.register("before_generate", () => {
  const themeConfig = hexo.theme.config;

  /**
   * Merge CDN
   */

  const defaultCDN = {
    main_css: "/css/index.css",
    main: "/js/main.js",
    utils: "/js/utils.js",

    // pjax
    pjax: "https://jsdelivr.pai233.top/npm/pjax/pjax.min.js",

    // comments
    gitalk: "https://jsdelivr.pai233.top/npm/gitalk@latest/dist/gitalk.min.js",
    gitalk_css: "https://jsdelivr.pai233.top/npm/gitalk/dist/gitalk.min.css",
    blueimp_md5: "https://jsdelivr.pai233.top/npm/blueimp-md5/js/md5.min.js",
    valine: "https://jsdelivr.pai233.top/npm/valine/dist/Valine.min.js",
    disqusjs: "https://jsdelivr.pai233.top/npm/disqusjs@1/dist/disqus.js",
    disqusjs_css:
      "https://jsdelivr.pai233.top/npm/disqusjs@1/dist/disqusjs.css",
    utterances: "https://utteranc.es/client.js",
    twikoo: "https://jsdelivr.pai233.top/npm/twikoo/dist/twikoo.all.min.js",
    waline: "https://jsdelivr.pai233.top/npm/@waline/client/dist/Waline.min.js",
    giscus: "https://giscus.app/client.js",

    // share
    addtoany: "https://static.addtoany.com/menu/page.js",
    sharejs:
      "https://jsdelivr.pai233.top/npm/social-share.js/dist/js/social-share.min.js",
    sharejs_css:
      "https://jsdelivr.pai233.top/npm/social-share.js/dist/css/share.min.css",

    // search
    local_search: "/js/search/local-search.js",
    algolia_js: "/js/search/algolia.js",
    algolia_search_v4:
      "https://jsdelivr.pai233.top/npm/algoliasearch@4/dist/algoliasearch-lite.umd.js",
    instantsearch_v4:
      "https://jsdelivr.pai233.top/npm/instantsearch.js@4/dist/instantsearch.production.min.js",

    // math
    mathjax: "https://jsdelivr.pai233.top/npm/mathjax@3/es5/tex-mml-chtml.js",
    katex: "https://jsdelivr.pai233.top/npm/katex@latest/dist/katex.min.css",
    katex_copytex:
      "https://jsdelivr.pai233.top/npm/katex@latest/dist/contrib/copy-tex.min.js",
    katex_copytex_css:
      "https://jsdelivr.pai233.top/npm/katex@latest/dist/contrib/copy-tex.css",
    mermaid: "https://jsdelivr.pai233.top/npm/mermaid/dist/mermaid.min.js",

    // count
    busuanzi: "//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js",

    // background effect
    canvas_ribbon:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/canvas-ribbon.min.js",
    canvas_fluttering_ribbon:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/canvas-fluttering-ribbon.min.js",
    canvas_nest:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/canvas-nest.min.js",

    lazyload:
      "https://jsdelivr.pai233.top/npm/vanilla-lazyload/dist/lazyload.iife.min.js",
    instantpage:
      "https://jsdelivr.pai233.top/npm/instant.page/instantpage.min.js",
    typed: "https://jsdelivr.pai233.top/npm/typed.js/lib/typed.min.js",
    pangu: "https://jsdelivr.pai233.top/npm/pangu/dist/browser/pangu.min.js",

    // photo
    fancybox_css_v4:
      "https://jsdelivr.pai233.top/npm/@fancyapps/ui/dist/fancybox.css",
    fancybox_v4:
      "https://jsdelivr.pai233.top/npm/@fancyapps/ui/dist/fancybox.umd.js",
    medium_zoom:
      "https://jsdelivr.pai233.top/npm/medium-zoom/dist/medium-zoom.min.js",

    // snackbar
    snackbar_css:
      "https://jsdelivr.pai233.top/npm/node-snackbar/dist/snackbar.min.css",
    snackbar:
      "https://jsdelivr.pai233.top/npm/node-snackbar/dist/snackbar.min.js",

    // effect
    activate_power_mode:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/activate-power-mode.min.js",
    fireworks:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/fireworks.min.js",
    click_heart:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/click-heart.min.js",
    ClickShowText:
      "https://jsdelivr.pai233.top/npm/butterfly-extsrc@1/dist/click-show-text.min.js",

    // fontawesome
    fontawesomeV6:
      "https://jsdelivr.pai233.top/npm/@fortawesome/fontawesome-free@6/css/all.min.css",

    // Conversion between Traditional and Simplified Chinese
    translate: "/js/tw_cn.js",

    // flickr-justified-gallery
    flickr_justified_gallery_js:
      "https://jsdelivr.pai233.top/npm/flickr-justified-gallery@2/dist/fjGallery.min.js",
    flickr_justified_gallery_css:
      "https://jsdelivr.pai233.top/npm/flickr-justified-gallery@2/dist/fjGallery.min.css",

    // aplayer
    aplayer_css: "https://jsdelivr.pai233.top/npm/aplayer/dist/APlayer.min.css",
    aplayer_js: "https://jsdelivr.pai233.top/npm/aplayer/dist/APlayer.min.js",
    meting_js:
      "https://jsdelivr.pai233.top/gh/metowolf/MetingJS@1.2/dist/Meting.min.js",

    // Prism.js
    prismjs_js: "https://jsdelivr.pai233.top/npm/prismjs/prism.min.js",
    prismjs_lineNumber_js:
      "https://jsdelivr.pai233.top/npm/prismjs/plugins/line-numbers/prism-line-numbers.min.js",
    prismjs_autoloader:
      "https://jsdelivr.pai233.top/npm/prismjs/plugins/autoloader/prism-autoloader.min.js",
  };

  // delete null value
  const deleteNullValue = (obj) => {
    for (const i in obj) {
      obj[i] === null && delete obj[i];
    }
    return obj;
  };

  themeConfig.CDN = Object.assign(defaultCDN, deleteNullValue(themeConfig.CDN));

  /**
   * Capitalize the first letter of comment name
   */

  let { use } = themeConfig.comments;

  if (!use) return;

  if (typeof use === "string") {
    use = use.split(",");
  }

  const newArray = use.map((item) =>
    item.toLowerCase().replace(/\b[a-z]/g, (s) => s.toUpperCase())
  );

  themeConfig.comments.use = newArray;
});
```

<font style="color:rgb(51, 51, 51);">替换完后，运行</font>

<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">hexo cl && hexo g -d</font>

<font style="color:rgb(51, 51, 51);">部署后，就成功切换到了你的反代源上。你也可以直接复制博主的成果进行使用~</font>
