新聞人報社社訊永久存檔網站 — 技術維護文件

本文件整理了網站開發與維護過程中遇到的問題與解決方式，作為後續維護參考。

1. 導覽列顯示錯誤

問題

首頁導覽列正常，但進入年份索引或年份頁時導覽列顯示錯誤（About、首頁、首頁）。

原因

年份頁使用 layout: year，未載入自訂的 header.html，導致 fallback 至 Minima 預設導覽列。

解決方式

建立 _includes/header.html 並自訂導覽列。

將 _layouts/year.html 改為使用 layout: default，以載入自訂 header。

2. CSS 檔案缺失

問題

找不到 main.css 或 main.scss，無法修改樣式。

原因

Minima 主題的 CSS 內建於 gem 中，不會出現在 repo。

解決方式

建立 assets/css/custom.css。

在 _includes/head.html 加入：

<link rel="stylesheet" href="{{ '/assets/css/custom.css' | relative_url }}">

將自訂樣式寫入 custom.css。

3. 年份頁統計數字

需求

在年份頁顯示該年度文章數量。

解決方式

在 _layouts/year.html 加入計算邏輯：

{% assign count = 0 %}
{% for post in site.posts %}
  {% if post.date | date: "%Y" == page.year %}
    {% assign count = count | plus: 1 %}
  {% endif %}
{% endfor %}

並在標題顯示：

<h1>{{ page.year }} 年文章（共 {{ count }} 篇）</h1>

4. About 頁面內容更新

解決方式

編輯 about.md 或 about/index.md，加入網站介紹、掃描期數與聯絡方式。

5. Footer 聯繫方式修改

解決方式

建立 _includes/footer.html：

<footer class="site-footer">
  <div class="wrapper">
    <p>聯繫本站維護人員：<a href="mailto:你的Email@example.com">DDY</a></p>
  </div>
</footer>

確保 _layouts/default.html 有：

{% include footer.html %}

6. 資料夾結構調整

問題

期數資料夾巢狀顯示（如 0406 裡面包含 0608）。

解決方式

使用終端機移動資料夾：

mv images/2005/0406/0608 images/2005/

使期數資料夾平行排列。

7. 文章顯示時間

解決方式

在 _layouts/post.html 加入：

<p class="post-date" style="color:#777;">發布時間：{{ page.date | date: "%Y-%m-%d %H:%M" }}</p>

8. Git 推送流程

標準流程

git add .
git commit -m "Update site layout and content"
git push

9. 維護建議

修改 layout 或 include 後應檢查全站是否正常。

建議建立 CHANGELOG.md 記錄版本變更。

若需擴充功能（熱門文章、月份索引、分類頁），可新增對應 layout。

本文件將持續更新，以確保網站維護流程清晰且可追蹤。
