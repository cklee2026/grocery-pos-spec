# SPEC.md - 小型商店SaaS POS系统

> Version: 1.0
> Status: 🟡 DRAFT
> Date: 2026-04-21

---

## 📋 Proposal

### 为什么做这个?
马来西亚很多小型杂货店(kedai runcit)还在用纸笔记账，找货麻烦，不知道还剩多少库存。这个系统帮他们：
- 快速记录销售 (scan / 输入商品 → 结账)
- 知道剩下多少货 (库存管理)
- 知道今天赚了多少钱 (每日小结)
- 一账号管理多家分店

### 解决什么问题?
- ✅ 不用手写receipt
- ✅ 知道哪些商品快缺货
- ✅ 知道今天销售额
- ✅ 多家分店统一看数据

---

## 🎯 Specs

### 核心功能

| # | 功能 | 优先级 | 说明 |
|---|------|--------|------|
| 1 | 商品管理 | P0 | 添加/编辑/删除商品 |
| 2 | 销售开单 | P0 | 选择商品 → 结账 → 扣库存 |
| 3 | 库存查看 | P0 | 看每个商品剩多少 |
| 4 | 销售记录 | P1 | 查看历史销售 |
| 5 | 日结报表 | P1 | 今日销售、利润 |
| 6 | 店铺管理 | P1 | 多家分店切换 |
| 7 | 用户管理 | P2 | 员工账号、权限 |

### 用户交互流程

┌─────────────────────────────────────────┐
│ 主菜单 │
│ ─── │
│ 1. 开单 (卖东西) │
│ 2. 库存 (看剩多少) │
│ 3. 商品管理 (新增/修改) │
│ 4. 销售记录 (历史) │
│ 5. 日结 (今日小结) │
│ 6. 店铺管理 (分店) │
│ 7. 设置 │
│ 0. 退出 │
└─────────────────────────────────────────┘


#### 开单流程 (功能1)
1. 输入商品条码/名称
2. 系统显示：商品名称、价格、库存
3. 输入数量
4. 继续加商品 or 结账
5. 显示总计，找零
6. 库存自动扣减


#### 库存查看 (功能2)
1. 显示所有商品列表
2. 标红显示：库存 < 10 的商品


#### 商品管理 (功能3)
1. 列表所有商品
2. 新增：输入条码、名称、售价、成本、库存
3. 编辑：修改任意字段
4. 删除：确认后删除


#### 销售记录 (功能4)
1. 显示今日销售列表
2. 可按日期筛选
3. 显示每单详情（买了什么）


#### 日结报表 (功能5)
1. 今日销售总额
2. 今日订单数量
3. 热销商品 top 5
4. 成本、利润


#### 店铺管理 (功能6)
1. 切换当前店铺
2. 查看各店销售额
3. 汇总所有店数据


### 数据模型

#### 商品 (Product)
{
 "id": "string",
 "barcode": "string",
 "name": "string",
 "category": "string",
 "cost_price": 0.00,
 "sell_price": 0.00,
 "quantity": 0,
 "min_stock": 10,
 "shop_id": "string"
}


#### 销售 (Sale)
{
 "id": "string",
 "date": "YYYY-MM-DD HH:MM",
 "items": [
 {"product_id": "string", "qty": 1, "price": 10.00}
 ],
 "subtotal": 0.00,
 "gst": 0.00,
 "total": 0.00,
 "cash": 0.00,
 "change": 0.00,
 "payment_method": "cash/card/touch_n_go",
 "shop_id": "string"
}


#### 店铺 (Shop)
{
 "id": "string",
 "name": "string",
 "address": "string",
 "code": "string"
}


#### 用户 (User)
{
 "id": "string",
 "username": "string",
 "password": "string",
 "role": "admin/cashier",
 "shop_id": "string"
}


### 马来西亚常用场景

| 场景 | 处理 |
|------|------|
| 顾客买了东西要receipt | 打印小票 |
| 不知barcode | 输入���称搜索 |
| 商品快缺货 | 黄色警告 |
| 赊账 (taugeh) | 记录顾客名 + 欠款 |
| 多币种收款 | Cash / Touch n Go / DuitNow |
| 多家分店 | 切换店铺查看 |

---

## 🎨 Design

### 技术栈
- 语言: Python 3.10+
- 界面: TUI (文字界面)
- 数据: SQLite (本地文件)
- 可选: Flask API (手机扫码)

### 项目结构
grocery_pos/
├── app.py # 主程序
├── database.py # 数据库操作
├── models.py # 数据模型
├── utils.py # 工具函数
├── data/
│ └── pos.db # SQLite 数据库
├── reports/ # 日结报表
└── tests/
 └── test.py # 测试


### 核心函数


### 预设商品数据 (Initial Data)
# 马来西亚杂货店常见商品
| 条码 | 名称 | 成本 | 售价 | 初始库存 |
|------|------|------|------|----------|
| 95560001 | Milo 袋装 (40g) | 2.50 | 3.00 | 50 |
| 95560002 | 汽水罐装 (355ml) | 1.50 | 2.00 | 100 |
| 95560003 | 面包 (plain) | 1.20 | 1.80 | 30 |
| 95560004 | 鸡蛋 (10粒) | 4.00 | 5.00 | 20 |
| 95560005 | 香烟 (小) | 11.00 | 13.00 | 10 |


### 预设店铺数据
| 代码 | 名称 | 地址 |
|------|------|------|
| HQ | 总店 | Lahad Datu |
| BR1 | 分店1 | Sandakan |
| BR2 | 分店2 | Tawau |


### SQLite 表结构

CREATE TABLE shops (
 id INTEGER PRIMARY KEY,
 code TEXT UNIQUE,
 name TEXT NOT NULL,
 address TEXT,
 created_at DATETIME DEFAULT CURRENT_TIMESTAMP);

CREATE TABLE products (
 id INTEGER PRIMARY KEY,
 barcode TEXT UNIQUE,
 name TEXT NOT NULL,
 category TEXT,
 cost_price REAL,
 sell_price REAL,
 quantity INTEGER,
 min_stock INTEGER DEFAULT 10,
 shop_id INTEGER,
 created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
 FOREIGN KEY(shop_id) REFERENCES shops(id));

CREATE TABLE users (
 id INTEGER PRIMARY KEY,
 username TEXT UNIQUE,
 password TEXT,
 role TEXT DEFAULT 'cashier',
 shop_id INTEGER,
 FOREIGN KEY(shop_id) REFERENCES shops(id));

CREATE TABLE sales (
 id INTEGER PRIMARY KEY,
 created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
 subtotal REAL,
 gst REAL,
 total REAL,
 cash REAL,
 `change` REAL,
 payment_method TEXT,
 shop_id INTEGER,
 user_id INTEGER,
 FOREIGN KEY(shop_id) REFERENCES shops(id),
 FOREIGN KEY(user_id) REFERENCES users(id));

CREATE TABLE sale_items (
 id INTEGER PRIMARY KEY,
 sale_id INTEGER,
 product_id INTEGER,
 qty INTEGER,
 price REAL,
 FOREIGN KEY(sale_id) REFERENCES sales(id),
 FOREIGN KEY(product_id) REFERENCES products(id));


---

## ✅ Tasks

### Phase 1: 基础
- [ ] 1.1 项目框架搭建
- [ ] 1.2 数据库初始化 (SQLite)
- [ ] 1.3 预设店铺数据
- [ ] 1.4 用户登录 (简单)

### Phase 2: 商品
- [ ] 2.1 商品 CRUD (增删改查)
- [ ] 2.2 条码搜索
- [ ] 2.3 名称模糊搜索
- [ ] 2.4 预设商品数据

### Phase 3: 销售
- [ ] 3.1 开单功能 (选择商品 + 数量)
- [ ] 3.2 结账 + 找零计算
- [ ] 3.3 GST 计算 (6%)
- [ ] 3.4 自动扣库存
- [ ] 3.5 打印小票 (可选)

### Phase 4: 库存
- [ ] 4.1 库存列表显示
- [ ] 4.2 低库存警告 (黄色标红)
- [ ] 4.3 库存手动调整

### Phase 5: 报表
- [ ] 5.1 销售记录查询
- [ ] 5.2 日结报表 (销售额、成本、利润)
- [ ] 5.3 热销商品排行
- [ ] 5.4 按日期筛选

### Phase 6: 多店
- [ ] 6.1 店铺切换
- [ ] 6.2 各店销售分开
- [ ] 6.3 汇总报表

### Phase 7: 用户 (可选)
- [ ] 7.1 员工账号
- [ ] 7.2 权限区分 (admin/cashier)
- [ ] 7.3 密码修改

### Phase 8: 测试 & 部署
- [ ] 8.1 基础功能测试
- [ ] 8.2 边界测试 (库存为0)
- [ ] 8.3 打包 exe/ipa

---

## 📦 Dependencies

# requirements.txt
flask>=2.0 # 如果用 Web 界面
sqlalchemy>=2.0 # ORM (可选)
pytest>=7.0 # 测试
fpdf>=1.7 # PDF 小票 (可选)


---

## 🎯 Acceptance Criteria

### 功能验收
- [ ] ���添加/编辑/删除商品
- [ ] 开单后库存自动扣减
- [ ] 输入不存在条码 → 提示找不到
- [ ] 库存为0 → 不能卖
- [ ] 日结显示销售额和利润
- [ ] 低库存显示警告
- [ ] 可切换不同店铺
- [ ] 各店数据分开

### 边缘情况
- [ ] 库存不足 → 提示库存不够
- [ ] 找零为负 → 提示钱不够
- [ ] 退出时未结账 → 确认放弃
- [ ] GST 计算正确

---

## ⚠️ 讨论阶段

以上是 Draft 版本，请问：
1. **有什么需要补充或修改的吗？**
2. **功能优先级需要调整吗？**
3. **还要加什么？**

确认后我标记 Final，推送 GitHub 🦞