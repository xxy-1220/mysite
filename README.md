# mysite

基于 **Django 5.2** 的 Web 开发学习项目，包含用户管理、成都天气查询、登录等多个示例功能，用于练习 Django 视图、模板、ORM 与路由等基础用法。

## 技术栈

- **后端框架**：Django 5.2.17
- **数据库**：MySQL（默认配置连接本地 `mysite2` 库）
- **前端**：Bootstrap 3.4.1 + jQuery 3.6.0
- **第三方库**：`requests`（天气接口调用）

## 项目结构

```
mysite/
├── manage.py              # Django 项目管理入口
├── db.sqlite3             # SQLite 空数据库文件（当前使用 MySQL，未启用）
├── mysite/                # 项目配置目录
│   ├── settings.py        # 全局配置（数据库、应用、模板、静态文件等）
│   ├── urls.py            # 根路由配置
│   ├── asgi.py / wsgi.py  # 部署入口
│   └── __init__.py
└── app01/                 # 业务应用
    ├── views.py           # 视图函数
    ├── models.py          # 数据模型（UserInfo、Department）
    ├── urls（由根路由引入） # 应用路由
    ├── admin.py / tests.py / apps.py
    ├── migrations/        # 数据库迁移文件
    ├── templates/         # 模板（login、weather、info_list 等）
    └── static/            # 静态资源（Bootstrap、jQuery、图片）
```

## 快速开始

### 1. 环境要求

- Python 3.10+
- MySQL 5.7 / 8.0（本地已启动）

### 2. 安装依赖

```bash
pip install django mysqlclient requests
```

> 若 `mysqlclient` 安装失败，可改用 `PyMySQL`，并在 `mysite/__init__.py` 中加入 `pymysql.install_as_MySQLdb()`。

### 3. 配置数据库

编辑 `mysite/settings.py` 中的 `DATABASES`，确认数据库信息与本地一致：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mysite2',        # 数据库名
        'USER': 'root',           # 用户名
        'PASSWORD': '123456',     # 密码
        'HOST': '127.0.0.1',
        'PORT': 3306,
    }
}
```

并确保已创建对应数据库：

```sql
CREATE DATABASE mysite2 DEFAULT CHARACTER SET utf8mb4;
```

### 4. 生成迁移并创建表

```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. 启动服务

```bash
python manage.py runserver
```

浏览器访问 `http://127.0.0.1:8000`。

## 功能与路由

| 路由 | 功能 | 说明 |
|------|------|------|
| `/index/` | 欢迎页 | 返回文本「欢迎使用」 |
| `/user/list/` | 用户列表页 | 渲染 `user_list.html` |
| `/user/add/` | 用户添加页 | 渲染 `user_add.html` |
| `/tpl/` | 模板语法演示 | 展示变量、列表、字典、循环 |
| `/weather/` | 成都天气 | 调用 open-meteo 接口并渲染 |
| `/something/` | 跳转百度 | 重定向到 `https://www.baidu.com` |
| `/login/` | 登录 | 账号 `root` / 密码 `123` 登录成功跳转联通官网 |
| `/orm/` | ORM 演示 | 练习 ORM 增删改查 |
| `/info/list/` | 用户信息列表 | 展示 `UserInfo` 全部记录 |
| `/info/add/` | 新增用户信息 | 通过 POST 表单写入数据库 |
| `/info/delete/` | 删除用户信息 | 按 `nid` 删除记录 |

## 数据模型（`app01/models.py`）

- **UserInfo**：用户信息（`name`、`password`、`age`）
- **Department**：部门（`title`）

## 注意事项

- `settings.py` 中 `DEBUG = True`，仅用于开发环境，请勿用于生产。
- `SECRET_KEY` 为开发用默认值，部署前请更换。
- `views.py` 顶部存在一行无实际用途的 `from matplotlib.pyplot import title` 导入，若未安装 matplotlib 且不使用该行，可将其删除避免报错。
