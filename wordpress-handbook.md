# WordPress 常见报错排查手册（作品样品）

> 90% 的 WordPress 故障都逃不出下面这 10 类。按顺序排查，大多数问题 30 分钟内可定位。

## 1. 白屏（White Screen of Death）

- **症状**：前台/后台一片空白，无任何报错
- **排查**：在 `wp-config.php` 加 `define('WP_DEBUG', true);` 看真实报错；八成是某个插件 PHP 报错
- **修复**：FTP 进 `wp-content/plugins`，逐个重命名插件文件夹，找到肇事插件后停用或替换

## 2. 500 Internal Server Error

- **症状**：整站 500
- **排查**：先查 `.htaccess` 是否损坏（重命名让 WP 自动重建）；再看 PHP 内存
- **修复**：`wp-config.php` 加 `define('WP_MEMORY_LIMIT', '256M');`；仍不行则逐个停用插件排查

## 3. 数据库连接错误（Error establishing a database connection）

- **症状**：整站提示数据库连接失败
- **排查**：检查 `wp-config.php` 中 DB_NAME / DB_USER / DB_PASSWORD / DB_HOST 是否正确；登录主机面板确认 MySQL 服务在运行
- **修复**：修正配置；若刚迁移过站点，八成是数据库信息没改对

## 4. 更新/安装插件时 429 或超时

- **症状**：后台装插件转圈后失败
- **排查**：多为国内服务器连 wordpress.org 超时
- **修复**：手动下载插件 zip 包，后台"上传安装"；或换国内镜像源

## 5. 固定链接 404（文章页打不开，首页正常）

- **症状**：点文章全是 404
- **排查**：伪静态规则丢失
- **修复**：后台"设置 → 固定链接"点一次保存；Nginx 服务器需手动配 rewrite 规则

## 6. 上传图片提示"HTTP 错误"

- **症状**：媒体库上传失败
- **排查**：PHP 上传大小限制、Imagick 缺失、或某图片优化插件冲突
- **修复**：调大 `upload_max_filesize`；停用图片优化类插件逐个测试

## 7. 后台巨慢 / 前台 TTFB 很高

- **症状**：打开页面要好几秒
- **排查**：用 Query Monitor 插件看是哪个插件/查询拖慢
- **修复**：装缓存插件（WP Rocket / LiteSpeed Cache）、开 OPcache、图片转 WebP

## 8. "Briefly unavailable for scheduled maintenance"

- **症状**：更新后一直卡在维护模式
- **排查**：更新中断，根目录残留 `.maintenance` 文件
- **修复**：FTP 删除根目录 `.maintenance` 文件即可

## 9. 忘记后台密码且邮箱收不到重置信

- **症状**：无法登录后台
- **排查**：服务器 mail() 函数被禁是常态
- **修复**：phpMyAdmin 进 `wp_users` 表，对管理员行用 MD5 函数设新密码；长期方案是接 SMTP 发信插件

## 10. 被挂马 / 跳转到博彩站

- **症状**：搜索自己网站出现博彩关键词，或访问跳转
- **排查**：查 `index.php`、主题 `functions.php` 是否被插入恶意代码；用 Wordfence 扫一遍
- **修复**：清理恶意代码 → 改所有密码（WP/FTP/数据库）→ 升级 WP/插件到最新 → 尽快上 WAF

---

*本手册为作品样品。接单时提供：故障诊断 → 报价确认 → 备份后修复 → 交付修复报告（含根因与预防建议）。*
