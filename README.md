# Git-alpha
WordPress博客程序：Git-alpha 开源主题

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
