# Goal-System

### database字段

- tbl_user
  - id(pk)
  - type 类型(0.普通用户 1.自媒体 2.管理员 3.超级管理员)
  - name
  - sex 性别（0.男 1.女）
  - birthday
  - telephone_number
  - email
  - address
  - create_time
  - create_by
  - update_time
  - update_by
  
- tbl_team
  - id(pk)
  - name
  - league_type（0.英超 1.西甲 2.德甲 3.意甲 4.法甲）
  - birthday
  - address
  - home_field
  - chief_coach
  - captain
  - description
  
- tbl_player
  - id(pk)
  - pid
  - name
  - age
  - stature
  - weight
  - birthday
  - nationality
  - value
  - position
  - number
  - contract_date
  - status 身体状况（0.健康 1.受伤）
  
- tbl_news
  
  - id(pk)
  
  - title
  - author
  - publish_time
  - content
  
- tbl_recode
  - id(pk)
  - type(0.申请 /1.点赞 /2.注册 /3.注销)
  - user_id
  - news_id
  - create_time
  - create_by
  - status(0.同意 /1.拒绝 /2.审核中)

### 各角色的权限

- 普通用户

  - 首页
    - 头条新闻
  - 比赛
  - 数据
    - 球队（球队，）
    - 球员（）
  - 我的
    - 关注
    - 资料
  
- 自媒体

  - 首页
    - 头条新闻
  
  - 比赛
  - 数据
    - 球队（球队，）
    - 球员（）
  - 发布
    - 编辑
    - 已发布
  - 我的
    - 关注
    - 资料
  
- 管理员
  - 首页
    - 头条新闻
  - 比赛
  - 数据
    - 球队
    - 球员
  - 管理
    - 用户管理
    - 新闻管理
  - 我的
    - 关注
    - 资料
  
- 超级管理员
  - 首页
    - 头条新闻
  - 比赛
  - 数据
    - 球队
    - 球员
  - 管理
    - 用户管理（序号，用户名，邮箱，角色，注册时间，操作）
    - 权限管理（序号，角色，权限描述，操作）
    - 新闻管理（序号，标题，作者，发布时间，点赞次数，操作）
    - 球队管理（世界排名，球队，本年积分，总身价，联赛，主场）
    - 球员管理
  - 我的
    - 关注
    - 资料
  - 日志





```sql
CONCAT(SUBSTR(tn.TITLE FROM 1 FOR 12),'...')
```