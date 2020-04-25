---
title:      Django migrate 重置
date:       2020-04-26
tags:
    - Django
    - migrate
---
#### 一、查看本地 migrate 历史
```bash
python manage.py showmigrations
```

```bash
action_backend
 [X] 0001_initial
 [X] 0002_auto_20180228_0136
 [X] 0003_magnetdata_reading_time
 [X] 0004_auto_20180302_0650
 [X] 0005_auto_20180510_0812
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
```

#### 二、重置
```bash
# fake 表示不实际执行操作
# zero 表示重置 action_backend 部分的数据库
python manage.py migrate --fake action_backend zero
```

#### 三、查看
```bash
python manage.py showmigrations
```

```bash
action_backend
 [ ] 0001_initial
 [ ] 0002_auto_20180228_0136
 [ ] 0003_magnetdata_reading_time
 [ ] 0004_auto_20180302_0650
 [ ] 0005_auto_20180510_0812
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
```

#### 四、删除 action_backend/migrations 文件夹下的文件，运行 makemigrations
```bash
python3 manage.py makemigrations
```

#### 五、使用 fake-initial 让 migrate 记录 action_backend 执行到 0001_initial.py，但并不会真的执行 0001_initial.py 里的代码
```bash
python3 manage.py migrate --fake-initial
```
