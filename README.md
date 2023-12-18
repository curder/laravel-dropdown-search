# Laravel dropdown search

## 初始化项目

```bash
# 下载项目
git clone git@github.com:curder/laravel-dropdown-search.git

# 配置数据库链接
cp .env.example .env

# 修改数据库连接信息
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_dropdown_search
DB_USERNAME=root
DB_PASSWORD=root

# 安装并启动meilisearch
# 查看这里: https://www.meilisearch.com/docs/learn/getting_started/installation

# 在本地.env文件中配置 meilisearch 信息
SCOUT_DRIVER=meilisearch
MEILISEARCH_HOST=http://127.0.0.1:7700
MEILISEARCH_KEY=

# 更新 PHP 依赖
composer install 

# 更新 User 和 Course 数据索引
php artisan scout:import "App\Models\User"
php artisan scout:import "App\Models\Course"

# 编译前端资源
cd learvel-dropdown-search
yarn && yarn build
```

## 启动项目

访问 http://laravel-dropdown-search.test/register 并注册一个测试用户。

然后在 http://laravel-dropdown-search.test/login 进行登录，来到 Dashboard 页面。

现在可以在 Dashboard 页面开始进行搜索了。

## 相关资源

- [Dropdown Autocomplete Search Anything in Laravel](https://codecourse.com/courses/dropdown-autocomplete-search-anything-in-laravel)
- [@algolia/autocomplete](https://github.com/algolia/autocomplete)
- [Meilisearch docs](https://www.meilisearch.com/docs)
