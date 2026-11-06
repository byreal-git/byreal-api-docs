Byreal DEX API V2/V3 接口文档
更新日期：2026-11-06
服务范围：byreal-dex-api-*
本文仅整理 API 服务中的 V2/V3 HTTP 接口，不包含 byreal-dex-admin-* 管理后台接口。

1. 公共约定
1.1 服务路径
类型	基础路径
DEX V2	/byreal/api/dex/v2
DEX V3	/byreal/api/dex/v3
Onchain V2	/byreal/api/onchain/v2
Onchain V3	/byreal/api/onchain/v3
1.2 链标识
chainId	chain	链
0	solana	Solana
5000	mantle	Mantle
未传 chainId 时，V3 查询通常表示查询全部已支持链；具体行为以各接口说明为准。

1.3 通用响应
除文件上传等特殊接口外，API 使用 DexResp<T> 作为统一响应结构。

{
  "success": true,
  "version": "v2.0.0",
  "timestamp": 1793952000000,
  "ret_code": 0,
  "ret_msg": null,
  "args": null,
  "data": {}
}
字段	类型	说明
success	Boolean	请求是否成功
version	String	API 响应版本
timestamp	Long	服务端时间戳，毫秒
ret_code	Integer	返回码，成功为 0
ret_msg	String	返回信息
args	Array	错误参数
data	Object	业务数据
2. V2 接口总览
当前代码共包含 56 个 V2 路由，其中 2 个标记为内部/测试接口。

2.1 Onchain
Method	Path	主要参数	用途
GET	/byreal/api/onchain/v2/pool/detail	poolAddresses[]	批量查询池子详情
GET	/byreal/api/onchain/v2/position/list	positionAddresses[]	批量查询仓位及奖励
GET	/byreal/api/onchain/v2/position/view/list	positionAddresses[]	批量查询仓位视图
GET	/byreal/api/onchain/v2/mint/list	tokenAddresses[]	批量查询 Token
2.2 通用配置与 Banner
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/main/jito-tip	-	查询 Jito Tip
GET	/byreal/api/dex/v2/main/auto-fee	-	查询自动优先费
GET	/byreal/api/dex/v2/main/config	-	查询前端公共配置
GET	/byreal/api/dex/v2/banner/list	-	查询旧 Banner 表中的在线 Banner
GET	/byreal/api/dex/v2/main/staking/bbsol/stats	-	查询 bbSOL 质押统计
2.3 Contest
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/contest/campaign/info	campaignType?	查询活动信息
GET	/byreal/api/dex/v2/contest/rank/list	TradeRankListReq Query	查询活动排行榜
GET	/byreal/api/dex/v2/contest/rank/my	MyTradeRankReq Query	查询当前用户排名
2.4 Copy Farmer
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/copyfarmer/overview	-	Copy Farmer 全局概览
POST	/byreal/api/dex/v2/copyfarmer/top-positions	FarmerPositionListReq JSON	查询 Top 仓位
POST	/byreal/api/dex/v2/copyfarmer/top-farmers	CopyFarmerProviderListReq JSON	查询 Top Farmer
GET	/byreal/api/dex/v2/copyfarmer/providerOverview	providerAddress	查询 Provider 详情
GET	/byreal/api/dex/v2/copyfarmer/epoch-bonus	walletAddress, type	查询 Epoch 奖励
GET	/byreal/api/dex/v2/copyfarmer/position-tvl-limit	-	查询仓位 TVL 限制配置
2.5 Hyper
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/symbols	-	查询 Hyper 交易对
2.6 Incentive
Method	Path	主要参数	用途
POST	/byreal/api/dex/v2/incentive/encode-v2	EncodeClaimRequest JSON	编码奖励领取交易 V2
POST	/byreal/api/dex/v2/incentive/encode-fee	EncodeFeeRequest JSON	编码手续费领取交易
POST	/byreal/api/dex/v2/incentive/encode-v3	EncodeClaimRequest JSON	编码奖励领取交易 V3
POST	/byreal/api/dex/v2/incentive/order-v2	ClaimRewardRequest JSON	提交奖励领取 V2
POST	/byreal/api/dex/v2/incentive/order-v3	ClaimRewardRequest JSON	提交奖励领取 V3
encode-v3 和 order-v3 虽以 v3 命名，但其完整路径仍属于 /byreal/api/dex/v2/incentive/**。

2.7 Kline
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/kline/query	tokenAddress, poolAddress, startTime?, endTime?, klineType	查询池子 K 线
GET	/byreal/api/dex/v2/kline/query-ui	同上	查询 UI 格式池子 K 线
GET	/byreal/api/dex/v2/chart/k-line	tokenAddress, startTime?, endTime?, klineType	查询 Token K 线
GET	/byreal/api/dex/v2/chart/k-line-ui	同上	查询 UI 格式 Token K 线
内部接口：GET /byreal/api/dex/v2/admin/k-line，不建议配置到公网 BGW。

2.8 Launchpad
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/launchpad/auction	auctionAddress	查询 Auction 详情
GET	/byreal/api/dex/v2/launchpad/list	-	查询 Auction 列表
GET	/byreal/api/dex/v2/launchpad/check/whitelist	auctionAddress, walletAddress	校验白名单
GET	/byreal/api/dex/v2/launchpad/whitelist/sign	auctionAddress, walletAddress	获取白名单签名
GET	/byreal/api/dex/v2/launchpad/user/commit/status	auctionAddress, walletAddress	查询用户认购状态
POST	/byreal/api/dex/v2/launchpad/send/order	SendLaunchpadTxReq JSON	提交认购订单
GET	/byreal/api/dex/v2/launchpad/statistic	-	查询 Launchpad 统计
2.9 Order
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/order/list	page, pageSize, userAddress	查询 Solana 用户订单
2.10 Pool
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/pools/info/list	PoolInfoListReq Query	查询 Solana 池子列表
GET	/byreal/api/dex/v2/pools/details	poolAddress	查询池子详情
GET	/byreal/api/dex/v2/overview/global	-	查询全局池子概览
POST	/byreal/api/dex/v2/pools/info/batch	String[] JSON	批量查询池子
2.11 Position
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/position/list	PositionListReq Query	查询 Solana 仓位列表
GET	/byreal/api/dex/v2/position/detail	address	查询仓位详情
GET	/byreal/api/dex/v2/position/overview	userAddress	查询仓位概览
POST	/byreal/api/dex/v2/liquidity/send	SendSwapTxReq JSON	发送流动性交易
GET	/byreal/api/dex/v2/position/unclaimed-data	userAddress	查询未领取数据
GET	/byreal/api/dex/v2/position/unclaimed-v2	userAddress	查询简化版未领取数据
2.12 Swap
Method	Path	主要参数	用途
POST	/byreal/api/dex/v2/send-swap-tx	SendSwapTxReq JSON	发送 Swap 交易
POST	/byreal/api/dex/v2/send-bbsol-tx	SendTxReq JSON	发送 bbSOL 交易
内部测试接口：GET /byreal/api/dex/v2/test/priority-fees，不建议配置到公网 BGW。

2.13 Token
Method	Path	主要参数	用途
GET	/byreal/api/dex/v2/mint/hot	type?	查询热门 Token
GET	/byreal/api/dex/v2/mint/list	TokenListReq Query	查询 Token 列表，固定 Solana
GET	/byreal/api/dex/v2/mint/price	mints[]?	批量查询 Token 价格
GET	/byreal/api/dex/v2/mint/ids	ids[]，最多 20 个	按 ID 批量查询 Token
GET	/byreal/api/dex/v2/mint/multiplier	mints[]	查询 Token multiplier
2.14 Upload
Method	Path	主要参数	用途
POST	/byreal/api/dex/v2/upload	file，Multipart	上传 PNG/JPEG 图片，最大 5 MB
3. V3 接口总览
当前代码共包含 36 个 V3 路由。

3.1 Onchain
Method	Path	主要参数	用途
GET	/byreal/api/onchain/v3/pool/detail	poolAddresses[], chainId?	批量查询多链池子详情
GET	/byreal/api/onchain/v3/position/list	positionAddresses[], chainId?	批量查询多链仓位
GET	/byreal/api/onchain/v3/position/view/list	positionAddresses[], chainId?	批量查询多链仓位视图
GET	/byreal/api/onchain/v3/mint/list	tokenAddresses[], chainId?	批量查询多链 Token
3.2 Banner
Method	Path	主要参数	用途
GET	/byreal/api/dex/v3/banner/list	-	查询新 Banner 表中已开启的 Banner
3.3 Copy Farmer
Method	Path	主要参数	用途
POST	/byreal/api/dex/v3/copyfarmer/top-positions	FarmerPositionListReq JSON	查询多链 Top 仓位
POST	/byreal/api/dex/v3/copyfarmer/top-farmers	CopyFarmerProviderListReq JSON	查询多链 Top Farmer
GET	/byreal/api/dex/v3/copyfarmer/overview	-	查询多链全局概览
POST	/byreal/api/dex/v3/copyfarmer/providerOverview	ProviderOverviewReq JSON	查询多链 Provider 概览
POST	/byreal/api/dex/v3/copyfarmer/epoch-bonus	EpochBonusReq JSON	查询多链 Epoch 奖励
POST	/byreal/api/dex/v3/copyfarmer/unclaim-view	UnClaimViewReq JSON	查询待领取奖励
3.4 Incentive
Method	Path	主要参数	用途
POST	/byreal/api/dex/v3/incentive/evm/encode712	EncodeClaimEvmRequest JSON	生成 EVM EIP-712 奖励领取签名
3.5 Kline
Method	Path	主要参数	用途
GET	/byreal/api/dex/v3/kline/pool-kline	tokenAddress, poolAddress, startTime?, endTime?, klineType, chain, multiplier?	查询多链池子 K 线
GET	/byreal/api/dex/v3/kline/token-kline	tokenAddress, startTime?, endTime?, klineType, chain, multiplier?	查询多链 Token K 线
3.6 Order
Method	Path	主要参数	用途
POST	/byreal/api/dex/v3/order/list	MultiChainOrderListRequest JSON	查询指定单链订单，当前只处理 owners[0]
POST	/byreal/api/dex/v3/order/merged-list	MergedOrderListRequest JSON	合并 Solana/Mantle 的 Swap 与 Bridge 订单
3.7 Pool
Method	Path	主要参数	用途
GET	/byreal/api/dex/v3/pools/info/list	PoolInfoListReq Query	查询多链池子列表
GET	/byreal/api/dex/v3/pools/info/listV3	PoolInfoListReqV3 Query	查询新版多链池子列表
GET	/byreal/api/dex/v3/pools/topData	-	查询池子 Top 数据
GET	/byreal/api/dex/v3/pools/details	poolAddress, chain?	查询池子详情
GET	/byreal/api/dex/v3/pools/ticks	poolAddress, chain	查询 EVM 池流动性深度
GET	/byreal/api/dex/v3/overview/global	-	查询多链全局概览
POST	/byreal/api/dex/v3/pools/info/batch	PoolRef[] JSON	批量查询多链池子
3.8 Position
Method	Path	主要参数	用途
POST	/byreal/api/dex/v3/position/list	MultiChainPositionListReq JSON	查询多链仓位列表
GET	/byreal/api/dex/v3/position/detail	address, chain?	查询仓位详情
GET	/byreal/api/dex/v3/position/overview	userAddress	查询仓位概览
GET	/byreal/api/dex/v3/position/unclaimed-data	userAddress	查询未领取数据
GET	/byreal/api/dex/v3/position/unclaimed-v2	userAddress	查询简化版未领取数据
POST	/byreal/api/dex/v3/position/unclaimed	MultiChainOwnersReq JSON	查询多链未领取数据
POST	/byreal/api/dex/v3/position/claimable/list	ClaimablePositionReq JSON	查询可领取仓位标识
3.9 Token
Method	Path	主要参数	用途
GET	/byreal/api/dex/v3/mint/hot	type?	查询热门 Token
GET	/byreal/api/dex/v3/mint/list	TokenListReq Query	查询多链 Token 列表
GET	/byreal/api/dex/v3/mint/queryList	TokenQueryListReq Query	按 Token 地址等条件查询列表
GET	/byreal/api/dex/v3/mint/price	mints[]?	批量查询 Token 价格
GET	/byreal/api/dex/v3/mint/ids	ids[]，最多 20 个	按 ID 批量查询 Token
GET	/byreal/api/dex/v3/mint/multiplier	mints[]	查询 Token multiplier
4. V2 与 V3 主要接口对照
业务	V2	V3	主要差异
Onchain 批量查询	/byreal/api/onchain/v2/**	/byreal/api/onchain/v3/**	V3 支持 chainId
Banner 列表	GET /byreal/api/dex/v2/banner/list	GET /byreal/api/dex/v3/banner/list	V3 使用新 Banner 表并返回 bannerConfig
Top 仓位	POST /copyfarmer/top-positions	POST /copyfarmer/top-positions	V3 支持多链
Top Farmer	POST /copyfarmer/top-farmers	POST /copyfarmer/top-farmers	V3 返回多链数据
Provider 概览	GET /copyfarmer/providerOverview	POST /copyfarmer/providerOverview	V3 使用 owners 请求体
Epoch 奖励	GET /copyfarmer/epoch-bonus	POST /copyfarmer/epoch-bonus	V3 使用 owners 请求体
池子列表	GET /pools/info/list	GET /pools/info/list	V2 固定 Solana，V3 支持多链
池子详情	GET /pools/details	GET /pools/details	V3 可传链标识
池子批量查询	POST /pools/info/batch	POST /pools/info/batch	V2 请求为地址数组，V3 请求为 PoolRef[]
仓位列表	GET /position/list	POST /position/list	V3 使用多链 owners 请求体
仓位详情	GET /position/detail	GET /position/detail	V3 可传 chain
Token 列表	GET /mint/list	GET /mint/list	V2 固定 Solana，V3 支持 chainId
Token 条件查询	-	GET /mint/queryList	V3 新增，最多传 5 个 Token 地址
订单列表	GET /order/list	POST /order/list	V3 使用 owner + chain 请求体
合并订单列表	-	POST /order/merged-list	V3 合并两链 Swap 和 Bridge 四路数据
池子 K 线	GET /kline/query、/query-ui	GET /kline/pool-kline	V3 支持多链和 multiplier
Token K 线	GET /chart/k-line、/k-line-ui	GET /kline/token-kline	V3 支持多链和 multiplier
表中省略的公共前缀：V2 为 /byreal/api/dex/v2，V3 为 /byreal/api/dex/v3。

5. 本次新增接口详情
5.1 Token 条件查询
基本信息
项	值
Method	GET
Path	/byreal/api/dex/v3/mint/queryList
Content-Type	application/json
返回类型	DexResp<PageResult<MintItem>>
Query 参数
参数	类型	必填	默认值	说明
tokenAddress	String[]	否	-	Token 地址列表，最多 5 个
page	Integer	否	1	页码，最小为 1
pageSize	Integer	否	10	每页数量，范围 1～500
sort	String	否	desc	排序方向：asc / desc
sortField	String	否	volumeUsd24h	排序字段
searchKey	String	否	-	地址精确查询或 Symbol 前缀查询
category	String	否	-	保留参数，当前实现未参与过滤
status	Integer	否	1	1=有效、0=无效、-1=全部
chainId	Integer	否	-	0=Solana、5000=Mantle，不传查询全部链
请求示例：

GET /byreal/api/dex/v3/mint/queryList?tokenAddress=So11111111111111111111111111111111111111112&chainId=0&page=1&pageSize=10
说明：

传入 tokenAddress 时使用一分钟本地缓存。
地址顺序不影响缓存 Key。
缓存最多保存 1000 个查询条件。
未传 tokenAddress 时复用普通 /mint/list 查询逻辑，不使用该缓存。
5.2 Banner V3 列表
基本信息
项	值
Method	GET
Path	/byreal/api/dex/v3/banner/list
请求参数	无
返回类型	DexResp<List<BannerV3Item>>
仅返回 enabled=2 的 Banner，排序规则为：

sortOrder DESC
createAt DESC
id DESC
返回业务字段：

字段	类型	说明
id	Long	数据库主键
bannerId	String	Banner 业务 ID
title	String	标题
enabled	Integer	状态，接口中固定为 2
sortOrder	Integer	排序权重，值越大越靠前
bannerConfig	Object	完整 Banner 业务配置 JSON
5.3 多链订单合并列表
基本信息
项	值
Method	POST
Path	/byreal/api/dex/v3/order/merged-list
Content-Type	application/json
返回类型	DexResp<MergedOrderListResponse>
请求体：

{
  "owners": [
    {
      "chain": "solana",
      "address": "11111111111111111111111111111111"
    },
    {
      "chain": "mantle",
      "address": "0x1111111111111111111111111111111111111111"
    }
  ]
}
参数	类型	必填	说明
owners	Array	是	数量 1～2；同一条链只能传一个地址
owners[].chain	String	是	solana / mantle，大小写不敏感
owners[].address	String	是	对应链的钱包地址
查询规则：

分别查询 Solana Swap、Mantle Swap、Solana Bridge、Mantle Bridge。
每路查询数量由 Nacos 配置 dex.order.merged-list.limit-per-source 控制。
每路默认 20 条，最小 1 条，最大 100 条。
两条链均传入时，默认最多返回 80 条。
四路订单统一转换为毫秒时间，并按时间倒序合并。
时间为空的记录排在最后；时间相同时按 id 升序。
Bridge 的 status 和 type 返回 null，原始状态见 bridge.orderStatus。
返回数据主体：

字段	类型	说明
total	Integer	本次四路合并后的实际数量
list[].source	String	SWAP / BRIDGE
list[].id	String	Swap 为 {chain}:{hash}；Bridge 为 bridge:{quoteId}
list[].input	Object	支付资产快照
list[].output	Object	到账资产快照
list[].owner	Object	钱包及所属链
list[].transaction	Object	交易链与交易哈希
list[].status	Integer	Swap 状态；Bridge 为 null
list[].type	Integer	0=AMM、1=RFQ；Bridge 为 null
list[].unixTime	Long	订单时间，Unix 毫秒
list[].routeInfo	Object	Swap 路由信息
list[].bridge	Object	Bridge 专有字段；Swap 不返回
6. 本次 BGW 新增配置清单
以下 3 个接口需要新增到 API 服务 BGW：

GET  /byreal/api/dex/v3/mint/queryList
GET  /byreal/api/dex/v3/banner/list
POST /byreal/api/dex/v3/order/merged-list
注意：

路径 /mint/queryList 中的 L 为大写，需保持完全一致。
/byreal/api/dex/v3/admin/banner-v3/** 属于 Admin 服务，不要加入 API 服务 BGW。
V2 原接口保持不变，本次无需新增 V2 BGW 路由。
7. 相关文档
DEX V3 多链接口详细文档
旧 Banner 接口文档
EVM V2 Token 列表差异说明
