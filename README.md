
# WordPress博客程序：Git-alpha 开源主题
<img width="1280" height="448" alt="image" src="https://github.com/user-attachments/assets/17da367b-09f9-4d2c-9907-36cca1f683ba" />

一、介绍：
这款主题是云落一直在使用的主题，是对欲思主题的二次修改版本，起名为 Git，是因为是使用 Git 来托管代码的，写得最多的就是 Git 了，所以就这么定名了。这款主题是我正式建站使用的主题，这款主题的发展一定程度上见证了云落从一个 WordPress 小白到如今的成长历史，所以将这款主题免费分享出来，希望朋友们能喜欢。

这款主题是在欲思主题的基础上进行改造的，我的博客用的也是这款主题，所以当时在搭建的过程中，遇得到了坑，网上的资料也比较少，就官网有文档，我在这里记录下搭建的过程。

详细的功能可参看官网的介绍：https://gitcafe.net  【官网已经没有了】

由于升级到PHP8.3主题出现好多错误，现在以修复正常使用，还有错误请留言。

二、功能：

Git-alpha 主题特色

兼容 IE9+、谷歌 Chrome、火狐 Firefox 等主流浏览器

扁平化 + 响应式设计，兼容电脑、平板和手机访问

主题设置面板新增多种广告位，PC 端和移动设备各不相同

自带 7 + 小工具，可随意设置侧栏分类和浮动块。

基本 SEO：首页、分类、文章等页面都可以设置关键词和描述

内置实用功能：Ajax 加载分页、垃圾广告拦截、缓存头像、社交账户图标等

短代码包括 dm 和 dl、gt 等, 可自主选择。


注意：本次所有修改均是为了兼容 PHP 8.0+：未初始化的变量、不存在的数组键、对 null 读属性、向类型严格函数传 null、可选参数在必选参数前——这些在旧版 PHP 中不报错，PHP 8+ 通通抛出 Warning/Deprecated 甚至 Fatal error。
1. admin/theme-options.php — 3 处修改
git_update_options(): 遍历选项时加 isset() 检查，跳过 type=title 的无键条目
$_GET 参数访问: $_GET['update']、$_GET['reset']、$_GET['test'] 加 isset() 保护
git_get_option(): 改为 isset() 检查数组键 + stripslashes 只在值存在时调用
2. include/avatar.php — 1 处修改
get_avatar() 方法签名: 移除 $size = 96 的可选默认值，修复 PHP 8.0+ 废弃的可选参数在必选参数之前的写法
3. functions.php — 3 处修改
deel_breadcrumbs(): get_the_category() 结果增加 empty()/is_wp_error() 检查；get_category_parents() 返回值增加 is_wp_error() 兜底
git_page_id(): $wpdb->get_row() 返回 null 时先行判空再取 ['post_id']
Git_Tax_Image::edit_tax_image_field(): get_option() 返回 false 时加 is_array() 判空
新增 git_fix_links_widget_css(): 用 CSS 修复友情链接小工具的描述和评分文本扰乱布局的问题
4. single.php — 1 处修改
面包屑分类: $category[0] 加 !empty() + isset() 前置判断
5. include/server.php — 2 处修改
baidu_record(): 函数签名改为 baidu_record($post_id = null)，补上漏掉的参数
Git_Baidu_Submit(): array_key_exists() 前加 is_array($result) 检查，防止 WP_Error 或 json_decode null 传入
6. admin/theme-widgets.php — 2 处修改
构造函数: $instance['title'] 加 !empty() 检查 + 默认值
widget() 方法: $output.= 改为 $output =（首次赋值）
7. include/seo.php — 2 处修改
tag_link(): 补上遗漏的 $case = 'i' 变量定义
deel_description(): 将 $post->post_excerpt 移到 is_singular() 块内，并加 isset() 检查
8. comments.php — 1 处修改
循环前加 $comment_i = 0; 初始化
9. pages/sitemap.php — 1 处修改
移除 wp_page_menu($args) 中未定义的 $args 参数

