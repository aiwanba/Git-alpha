
# WordPress博客程序：Git-alpha 开源主题

<img width="1400" height="491" alt="image" src="https://github.com/user-attachments/assets/03ed679b-2237-4c97-b1e7-eff9bad63a5c" />


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

## 📁 项目概览
Git-alpha 是一款由云落基于 yusi 主题二次开发的 WordPress 主题（版本 12.3.1），具备丰富的自定义能力和扩展功能。

## 🏗️ 核心架构
### 文件结构
## 🧩 核心功能模块
### 1. 主题入口 (functions.php)
作为主题的核心入口文件，负责：

- PHP 版本检查 （要求 PHP 5.5+）
- 常量定义 （GIT_VER, GIT_URL）
- 模块加载 （后台选项、小工具、元框、功能函数、积分系统）
- 主题特性注册 （缩略图、自定义背景、菜单）
- 全局功能 （分页、阅读量统计、点赞、评论样式、AJAX 处理）
### 2. 后台管理模块 (admin/)
文件 功能 theme-options.php 主题选项配置页面，支持搜索筛选 options-config.php 选项配置定义 theme-widgets.php 自定义小工具 theme-metabox.php 文章元数据框 theme-ajax.php 后台 AJAX 处理

### 3. 核心功能模块 (include/)
文件 功能 func_load.php 模块加载入口 shortcode.php 短代码系统 （下载、回复可见、密码可见、付费可见、媒体嵌入等） optimization.php 性能优化 （移除冗余代码、禁用 REST API、缓存、图片处理） avatar.php 头像功能 user.php 用户管理 （注册、登录、权限、字段扩展） seo.php SEO 优化 email.php 邮件功能 server.php 第三方服务集成

### 4. 扩展功能模块 (modules/)
文件 功能 points.php 积分系统 （金币管理、充值、消费） cms.php CMS 模式首页 sticky.php 3D 幻灯片轮播 slick.php 平面幻灯片轮播 related.php 相关文章 card.php 卡片式文章展示 excerpt.php 摘要式文章展示 payjs.php PayJS 支付集成 push.php 支付回调处理

### 5. 自定义页面 (pages/)
文件 功能 weauth.php 微信扫码登录 chongzhi.php 积分充值页面 download.php 下载页面 product.php 产品页面 shuoshuo.php 说说页面 tougao.php 投稿页面 readers.php 读者页面 daohang.php 导航页面

## 🎯 核心业务逻辑
### 1. 积分系统流程
关键函数：

- pay_chongzhi() - 充值请求处理
- pay_buy() - 付费内容购买
- Points::get_user_total_points() - 获取用户积分
- Points::set_points() - 设置积分
### 2. 内容权限控制
短代码 功能 [reply] 回复后可见 [vip] 登录后可见 [secret] 密码验证可见 [pax] 付费可见（免登录）

### 3. 评论防垃圾机制
- 语言过滤 ：强制中文评论，拒绝日文
- 关键词过滤 ：屏蔽敏感词、URL、IP
- 链接屏蔽 ：禁止昵称和评论内容带链接
- 长链接检测 ：URL 超过 50 字符标记为垃圾
### 4. 性能优化策略
- 小工具缓存 （18000秒）
- HTML 压缩
- 静态资源版本移除
- 图片懒加载
- CDN 支持 （七牛云）
- 禁用冗余功能 （REST API、表情、自动保存）
### 5. 用户系统增强
- 中文用户名支持
- 注册表单增强 （密码确认、长度验证）
- 后台数学验证码
- IP 注册限制
- 用户字段扩展 （QQ、微博、GitHub 等）
- 微信扫码登录
## 🎨 主题特色功能
1. 多皮肤支持 ：7种预设颜色 + 自定义颜色
2. 响应式设计 ：移动端适配
3. 文章阅读量统计
4. 文章点赞功能 （防重复）
5. 百度分享集成
6. 代码高亮 （多种主题）
7. 图片灯箱效果 （Fancybox）
8. 自定义表情
9. 面包屑导航
10. 自动更新服务
## 🔧 技术栈
- PHP （WordPress 主题开发）
- jQuery （前端交互）
- FontAwesome （图标库）
- PayJS （支付集成）
- Qrious （二维码生成）
- SweetAlert （弹窗提示）
## 📋 总结
Git-alpha 是一款功能全面的 WordPress 主题，核心优势在于：

1. 强大的内容变现能力 ：积分系统 + 付费内容 + 充值功能
2. 完善的用户管理 ：微信登录、注册增强、权限控制
3. 丰富的内容展示 ：卡片/摘要模式、幻灯片、相关文章
4. 全面的防垃圾机制 ：评论过滤、验证码、IP限制
5. 性能优化 ：缓存、压缩、CDN、懒加载
主题采用模块化设计，通过 git_get_option() 函数统一管理配置项，扩展性强，适合搭建内容付费型网站或个人博客。