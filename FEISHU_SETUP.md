# 飞书竞品数据源填写说明

这次要对接的飞书多维表格是：

`https://bwi7u5ha9he.feishu.cn/base/XzhrbVLaXaFcENslLujcOKrcnib`

其中这份 base 的 `app_token` 就是：

`XzhrbVLaXaFcENslLujcOKrcnib`

这个页面不是直接在 HTML 里手填竞品信息，而是通过：

`飞书多维表格 -> Node 接口 -> preview.html`

页面请求地址是：

`/api/competitor?grade=新高一`

后端代码在 [server.js](./server.js)，页面在 [preview.html](./preview.html)。

## 1. 先填写 .env

参考 [`.env.example`](./.env.example) 新建一个 `.env`，填写下面几项：

```env
PORT=3001

FEISHU_APP_ID=你的飞书应用 App ID
FEISHU_APP_SECRET=你的飞书应用 App Secret
FEISHU_APP_TOKEN=XzhrbVLaXaFcENslLujcOKrcnib
FEISHU_COMPETITOR_PRODUCTS_TABLE_ID=competitor_products 这张表的 table_id
FEISHU_ADVANTAGE_BLOCKS_TABLE_ID=advantage_blocks 这张表的 table_id
```

也就是说，你这次只需要再补：

- `FEISHU_APP_ID`
- `FEISHU_APP_SECRET`
- `FEISHU_COMPETITOR_PRODUCTS_TABLE_ID`
- `FEISHU_ADVANTAGE_BLOCKS_TABLE_ID`

## 2. 表一：competitor_products

这张表存顶部竞品对比表的数据。

建议表名就叫：

`competitor_products`

建议每一行代表一个品牌在某个年级下的一套产品。

### 推荐字段

| 字段名 | 是否必填 | 示例 | 说明 |
| --- | --- | --- | --- |
| `grade` | 是 | `新高一` | 支持 `新高一`、`新高二`、`新高三` |
| `brand` | 是 | `有道领世` | 表头品牌名 |
| `class_name` | 是 | `新高一半年卡（赠冲刺）` | 班型名称 |
| `product_content` | 是 | `暑：10节直播\n秋：16节直播+40节视频` | 产品内容，支持换行 |
| `live_lesson_count` | 是 | `26` | 直播课节数 |
| `live_lesson_duration` | 是 | `2h` | 每节直播时长 |
| `video_lesson_count` | 是 | `40` | 视频课节数 |
| `total_class_hours` | 是 | `144` | 总课时量 |
| `single_subject_price` | 是 | `3980` | 单科价格 |
| `single_subject_hourly_price` | 是 | `28` | 单科价格 / 每课时 |
| `combo_price` | 是 | `10140` | 联报价格 |
| `combo_hourly_price` | 是 | `23` | 联报价格 / 每课时 |
| `is_youdao` | 建议 | `true` | 是否高亮成有道领世列 |
| `badge` | 否 | `高性价比` | 品牌头部小标签 |
| `content_badge` | 否 | `唯一含知识视频` | 产品内容下面的橙色标签 |
| `sort_order` | 建议 | `1` | 控制品牌列顺序 |

## 3. 表二：advantage_blocks

建议表名就叫：

`advantage_blocks`

这张表存下面几个说明模块：

`note`
`product_composition`
`price_advantage`
`knowledge_video`
`live_method`
`tutoring_service`
`price_card`

建议每个 `grade + block_key` 对应一行。

## 4. 使用方式

1. 启动服务：`npm install && npm start`
2. 打开：`http://localhost:3001`
3. 修改飞书表内容
4. 刷新页面

## 5. 注意

- 不要用 `file://.../preview.html` 打开页面，这样不会请求本地 API。
- 一定要通过 `http://localhost:3001` 访问。
- 如果页面出现“数据加载失败，请稍后刷新”，优先检查：
  - `.env` 是否已填写
  - 飞书应用权限是否已开通
  - 两张表的 `table_id` 是否正确
  - 记录里的 `grade` 是否写成了 `新高一 / 新高二 / 新高三`
