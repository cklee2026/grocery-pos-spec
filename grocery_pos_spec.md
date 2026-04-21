# SPEC.md - 小型商店SaaS POS系统

> Version: 1.0
> Status: ✅ FINAL
> Date: 2026-04-21

---

## 📋 Proposal

### 为什么做这个?
马来西亚很多小型杂货店(kedai runcit)还在用纸笔记账，找货麻烦，不知道还剩多少库存。这个系统帮他们：
- 快速记录销售 (scan / 输入商品 → 结账)
- 知道剩下多少货 (库存管理)
- 知道今天赚了多少钱 (每日小结)
- 一账号管理多家分店
- 支持3语 (英文/中文/马来文)

### 解决什么问题?
- ✅ 不用手写receipt
- ✅ 知道哪些商品快缺货
- ✅ 知道今天销售额
- ✅ 多家分店统一看数据
- ✅ 3语切换 (英文/中文/马来文)
- ✅ 显示 RM 货币

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
| 8 | 语言设置 | P2 | 英文/中文/马来文 |
| 9 | 货币设置 | P2 | RM (马来西亚令吉) |
| 10 | 订阅管理 | P2 | 免费100SKU / 付费RM20 |
| 11 | 计费触发 | P2 | 超100SKU自动提醒续费 |
| 12 | 注册验证 | P2 | Email+Captcha+手动WhatsApp |
| 13 | 商店信息 | P2 | 店名/地址/电话 (Receipt显示) |
| 14 | 折扣 | P1 | -percent / 固定折扣 |
| 15 | 退货 | P1 | 退货/退款处理 |
| 16 | 赊账 | P1 | 顾客欠款记录 (taugeh) |
| 17 | 商品分类 | P2 | 快消/食品/烟酒分类 |
| 18 | 数据导出 | P2 | CSV导出备份 |

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

#### 注册流程 (新用户首次)
1. 打开网页 → 填Email + 商店名 + 地址 + 电话
2. Google Captcha 验证
3. 系统 → 状态: pending
4. 你 WhatsApp 发验证码给他
5. 他输入验证码 → 激活
6. 正常登录使用

#### Receipt 显示内容
- 店名 (shop_name)
- 地址 (shop_address)
- 电话 (shop_phone)


#### 开单流程 (功能1)
1. 输入商品条码 OR 扫描 barcode
2. 系统显示：商品名称、价格、库存
3. 输入数量
4. 继续加商品 or 结账
5. 显示总计，找零
6. 库存自动扣减

#### 商品搜索
- 方式1: 扫描 barcode (扫码枪)
- 方式2: 输入名称搜索 (模糊)
- 方式3: 输入部分条码


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

#### 语言设置 (功能8)
- Default: 英文
- 可选: 中文 / 马来文
- 切换后整个界面更新
- 菜单、按钮、提示语全切换

#### 货币设置 (功能9)
- 默认: RM (马来西亚令吉)
- 格式: RM 10.00
- Receipt 显示: RM XX.XX

#### 折扣 (功能14)
- 方式1: % 折扣 (5%, 10%, 15%, 20%)
- 方式2: 固定金额 (RM X)
- 结账前应用
- Receipt 显示折扣后金额

#### 退货 (功能15)
- 原 receipt 查找
- 选择退货商品 + 数量
- 退款方式：现金/原支付方式
- 库存加回

#### 赊账 (功能16)
- 记录顾客名 + 电话
- 记录欠款金额
- 列表显示欠款人
- 还钱时勾掉

#### 商品分类 (功能17)
- 默认分类：快消品、食品、烟酒、日用
- 报表可按分类统计

#### 数据导出 (功能18)
- 销售记录 CSV
- 商品列表 CSV
- 备份用


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

#### 设置 (Settings)
{
 "language": "en", // en/zh/ms
 "currency": "RM",
 "shop_id": "string"
}

#### 注册信息 (Registration - 必填)
{
 "shop_name": "string", // 店名 (Receipt显示)
 "shop_address": "string", // 地址 (Receipt显示)
 "shop_phone": "string", // 电话 (Receipt显示 + 验证用)
 "verified": false,
 "verify_code": "string",
 "verify_date": "YYYY-MM-DD"
}

#### 赊账 (Debt)
{
 "id": "string",
 "customer_name": "string",
 "customer_phone": "string",
 "amount": 0.00,
 "sale_id": "string",
 "status": "unpaid/paid",
 "created_at": "YYYY-MM-DD"
}

#### 退货 (Return)
{
 "id": "string",
 "original_sale_id": "string",
 "product_id": "string",
 "qty": 0,
 "refund_amount": 0.00,
 "reason": "string",
 "created_at": "YYYY-MM-DD"
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
- 界面: TUI (文字界面) + Web UI
- 数据: Cloudflare D1 (SQLite 云数据库)
- API: Cloudflare Workers (可选)
- 部署: Cloudflare Workers + Durable Objects

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


### Cloudflare 配置
| 服务 | 用途 | 费用 |
|------|------|------|
| D1 | 数据库 (SQLite) | 免费 5GB |
| Workers | API | 每月免费 100K 请求 |
| R2 | 文件存储 | 免费 1GB |
| KV | Key-Value 缓存 | 免费 1GB |

### 预设店铺数据
| 代码 | 名称 | 地址 |
|------|------|------|
| HQ | 总店 | Lahad Datu |
| BR1 | 分店1 | Sandakan |
| BR2 | 分店2 | Tawau |

### 定价模式 (SaaS)
| SKU 数量 | 月费 |
|------|------|
| ≤ 100 | 免费 |
| 101+ | RM 10/月 |

### 付费流程
1. 用户点击升级 → 显示银行账号
2. 用户转账 RM10 → 传 slip给你
3. 你手动确认 → 激活付费
4. 自动化：量上来再做 Stripe/DuitNow

### 订阅表 (Subscription)
{
 "shop_id": "string",
 "sku_count": 0,
 "plan": "free/premium",
 "start_date": "YYYY-MM-DD",
 "expiry_date": "YYYY-MM-DD",
 "status": "active/expired"}

### 翻译字符串 (i18n)
| Key | 英文 | 中文 | 马来文 |
|------|------|------|------|
| menu_main | Main Menu | 主菜单 | Menu Utama |
| menu_sale | Sale | 开单 | Jual |
| menu_stock | Stock | 库存 | Stok |
| menu_product | Product | 商品 | Produk |
| menu_report | Report | 报表 | Laporan |
| menu_shop | Shop | 店铺 | Kedai |
| menu_settings | Settings | 设置 | Tetapan |
| btn_checkout | Checkout | 结账 | Bayar |
| btn_cancel | Cancel | 取消 | Batal |
| lbl_total | Total | 总计 | Jumlah |
| lbl_change | Change | 找零 | Baki |
| lbl_stock | Stock | 库存 | Stok |
| msg_low_stock | Low Stock! | 库存不足! | Stok Rendah! |


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
 email TEXT UNIQUE,
 password TEXT,
 shop_name TEXT,
 shop_address TEXT,
 shop_phone TEXT,
 verified BOOLEAN DEFAULT 0,
 verify_code TEXT,
 verify_date DATETIME,
 role TEXT DEFAULT 'cashier',
 created_at DATETIME DEFAULT CURRENT_TIMESTAMP);

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
 discount_percent REAL DEFAULT 0,
 discount_amount REAL DEFAULT 0,
 FOREIGN KEY(sale_id) REFERENCES sales(id),
 FOREIGN KEY(product_id) REFERENCES products(id));

CREATE TABLE debts (
 id INTEGER PRIMARY KEY,
 customer_name TEXT,
 customer_phone TEXT,
 amount REAL,
 sale_id INTEGER,
 status TEXT DEFAULT 'unpaid',
 created_at DATETIME DEFAULT CURRENT_TIMESTAMP);

CREATE TABLE returns (
 id INTEGER PRIMARY KEY,
 original_sale_id INTEGER,
 product_id INTEGER,
 qty INTEGER,
 refund_amount REAL,
 reason TEXT,
 created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
 FOREIGN KEY(original_sale_id) REFERENCES sales(id),
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
- [ ] 3.2 条码扫描输入
- [ ] 3.3 名称搜索 (模糊)
- [ ] 3.4 折扣应用 (% / 固定)
- [ ] 3.5 结账 + 找零计算
- [ ] 3.6 GST 计算 (6%)
- [ ] 3.7 自动扣库存
- [ ] 3.8 打印小票 (可选)

### Phase 3.5: 退货
- [ ] 3.9 退货功能
- [ ] 3.10 退款处理
- [ ] 3.11 库存加回

### Phase 4: 库存
- [ ] 4.1 库存列表显示
- [ ] 4.2 低库存警告 (黄色标红)
- [ ] 4.3 库存手动调整

### Phase 4.5: 赊账
- [ ] 4.4 记录赊账
- [ ] 4.5 查看欠款人列表
- [ ] 4.6 标记已还

### Phase 5: 报表
- [ ] 5.1 销售记录查询
- [ ] 5.2 日结报表 (销售额、成本、利润)
- [ ] 5.3 热销商品排行
- [ ] 5.4 按日期筛选
- [ ] 5.5 分类统计

### Phase 5.5: 导出
- [ ] 5.6 CSV 销售导出
- [ ] 5.7 CSV 商品导出

### Phase 6: 多店
- [ ] 6.1 店铺切换
- [ ] 6.2 各店销售分开
- [ ] 6.3 汇总报表

### Phase 7: 用户 (可选)
- [ ] 7.1 员工账号
- [ ] 7.2 权限区分 (admin/cashier)
- [ ] 7.3 密码修改

### Phase 8: 订阅
- [ ] 8.1 计费逻辑 (≤100SKU免费)
- [ ] 8.2 超额提醒 (101+触发)
- [ ] 8.3 订阅状态检查

### Phase 9: 用户注册
- [ ] 9.1 注册页面 (Email + 商店信息)
- [ ] 9.2 Google Captcha
- [ ] 9.3 手动 WhatsApp 验证码
- [ ] 9.4 激活逻辑

### Phase 10: 设置
- [ ] 10.1 语言切换 (英文/中文/马来文)
- [ ] 10.2 货币设置 (RM)
- [ ] 10.3 界面翻译更新

### Phase 11: 测试 & 部署
- [ ] 11.1 基础功能测试
- [ ] 11.2 边界测试 (库存为0)
- [ ] 11.3 打包 exe/ipa

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