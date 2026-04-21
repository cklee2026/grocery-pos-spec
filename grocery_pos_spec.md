# SPEC.md - 小型杂货店POS系统

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

### 解决什么问题?
- ✅ 不用手写receipt
- ✅ 知道哪些商品快缺货
- ✅ 知道今天销售额

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

### 用户交互流程

```
┌─────────────────────────────────────────┐
│  主菜单                                │
│  ───                                  │
│  1. 开单 (卖东西)                     │
│  2. 库存 (看剩多少)                   │
│  3. 商品管理 (新增/修改)               │
│  4. 销售记录 (历史)                   │
│  5. 日结 (今日小结)                  │
│  0. 退出                              │
└─────────────────────────────────────────┘
```

#### 开单流程 (功能1)
```
1. 输入商品条码/名称
2. 系统显示：商品名称、价格、库存
3. 输入数量
4. 继续加商品 or 结账
5. 显示总计，找零
6. 库存自动扣减
```

#### 库存查看 (功能2)
```
1. 显示所有商品列表
2. 标红显示：库存 < 10 的商品
```

### 数据模型

#### 商品 (Product)
```json
{
  "id": "string",
  "barcode": "string",
  "name": "string",
  "category": "string",
  "cost_price": 0.00,
  "sell_price": 0.00,
  "quantity": 0,
  "min_stock": 10
}
```

#### 销售 (Sale)
```json
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
  "change": 0.00
}
```

### 马来西亚杂货店常用场景

| 场景 | 处理 |
|------|------|
| 顾客买了东西要receipt | 打印小票 |
| 不知barcode | 输入名称搜索 |
| 商品快缺货 | 黄色警告 |
| 赊账 (taugeh) | 记录顾客名 + 欠款 |
| 多币种收款 | 可以收 Cash / Touch n Go |

---

## 🎨 Design

### 技术栈
- **语言**: Python 3.10+
- **界面**: TUI (文字界面) 或 Simple Web
- **数据**: SQLite (本地文件)
- **可选**: Flask API (如果有手机扫码需求)

### 项目结构
```
grocery_pos/
├── app.py              # 主程序
├── database.py        # 数据库操作
├── models.py         # 数据模型
├── utils.py          # 工具函数
├── data/
│   └── pos.db        # SQLite 数据库
├── reports/         # 日结报表
└── tests/
    └── test.py     # 测试
```

### 核心函数

```python
# database.py
def init_db():
    """初始化数据库，创建表"""

def add_product(product: dict):
    """添加商品"""

def get_product(barcode: str):
    """查询商品"""

def update_stock(barcode: str, qty_change: int):
    """更新库存 (+/-)"""

def create_sale(items: list):
    """创建销售，扣库存"""

def get_daily_report(date: str):
    """获取日报表"""
```

```python
# app.py (主菜单)
def main_menu():
    while True:
        print("1. 开单 2. 库存 3. 商品 4. 记录 5. 日结 0. 退出")
        choice = input(">> ")
        
        if choice == "1": sale_menu()
        elif choice == "2": stock_menu()
        elif choice == "3": product_menu()
        elif choice == "4": history_menu()
        elif choice == "5": daily_report()
        elif choice == "0": break
```

### 预设商品数据 (Initial Data)
```
# 马来西亚杂货店常见商品
| 条码 | 名称 | 成本 | 售价 | 初始库存 |
|------|------|------|------|----------|
| 95560001 | Milo 袋装 (40g) | 2.50 | 3.00 | 50 |
| 95560002 | 汽水罐装 (355ml) | 1.50 | 2.00 | 100 |
| 95560003 | 面包 (plain) | 1.20 | 1.80 | 30 |
| 95560004 | 鸡蛋 (10粒) | 4.00 | 5.00 | 20 |
| 95560005 | 香烟 (小) | 11.00 | 13.00 | 10 |
```

### SQLite 表结构

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    barcode TEXT UNIQUE,
    name TEXT NOT NULL,
    category TEXT,
    cost_price REAL,
    sell_price REAL,
    quantity INTEGER,
    min_stock INTEGER DEFAULT 10,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE sales (
    id INTEGER PRIMARY KEY,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    subtotal REAL,
    gst REAL,
    total REAL,
    cash REAL,
    `change` REAL
);

CREATE TABLE sale_items (
    id INTEGER PRIMARY KEY,
    sale_id INTEGER,
    product_id INTEGER,
    qty INTEGER,
    price REAL,
    FOREIGN KEY(sale_id) REFERENCES sales(id),
    FOREIGN KEY(product_id) REFERENCES products(id)
);
```

---

## ✅ Tasks

### Phase 1: 基础
- [ ] 1.1 项目框架搭建
- [ ] 1.2 数据库初始化 (SQLite)
- [ ] 1.3 商品 CRUD (增删改查)
- [ ] 1.4 预设商品数据

### Phase 2: 销售
- [ ] 2.1 开单功能 (选择商品 + 数量)
- [ ] 2.2 结账 + 找零计算
- [ ] 2.3 自动扣库存
- [ ] 2.4 打印小票 (可选)

### Phase 3: 库存
- [ ] 3.1 库存列表显示
- [ ] 3.2 低库存警告 (黄色标红)
- [ ] 3.3 库存手动调整

### Phase 4: 报表
- [ ] 4.1 销售记录查询
- [ ] 4.2 日结报表 (销售额、成本、利润)
- [ ] 4.3 月结报表 (可选)

### Phase 5: 测试 & 部署
- [ ] 5.1 基础功能测试
- [ ] 5.2 边界测试 (库存为0)
- [ ] 5.3 在树莓派运行测试 (可选)

---

## 📦 Dependencies

```
# requirements.txt
flask>=2.0          # 如果用 Web 界面
sqlalchemy>=2.0      # ORM (可选)
pytest>=7.0          # 测试
fpdf>=1.7           # PDF 小票 (可选)
```

---

## 🎯 Acceptance Criteria

### 功能验收
- [ ] 能添加/编辑/删除商品
- [ ] 开单后库存自动扣减
- [ ] 输入不存在条码 → 提示找不到
- [ ] 库存为0 → 不能卖
- [ ] 日结显示销售额和利润
- [ ] 低库存显示警告

### 边缘情况
- [ ] 库存不足 → 提示库存不够
- [ ] 找零为负 → 提示钱不够
- [ ] 退出时未结账 → 确认放弃

---

**状态**: 🟡 等待确认 → 可以开始实现