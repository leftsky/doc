以太坊的丛生与便捷应用
=============

> 以太坊。区块链2.0。智能合约。

区块链-以太坊。一个 极具落地价值 却 被万众代币托起 的 分布式超级计算机。
从始未终，你或多或少有些理由到这，交流以太坊的开发与实现、接入与交易。让我们用`PHP`开始。

[扩展包下载](https://left-bbs.oss-cn-hangzhou.aliyuncs.com/eth_php_vendor.zip) 
或者 composer 安装 `composer require leftsky/eth_php`

```
// 创建钱包
$wallet = new Wallet();
// 主动设置钱包地址
//$wallet->address = '';
// 导入钱包
//$wallet = new Wallet($prv_key);

// 如果你的服务器在国内 （不推荐使用国内服务器
//        $wallet->config([
//            'etherscan_domain' => "http://api-cn.etherscan.com/api?"
//        ]);

// 打印钱包地址
echo "钱包地址：{$wallet->address}\n";
// 打印钱包私钥
echo "钱包私钥：{$wallet->prv_key}\n";
// 打印以太坊余额
echo "ETH 余额：{$wallet->balance()}\n";
// 打印代币余额
echo "代币 余额：{$wallet->balance("填入代币合约地址")}\n";
// 发送以太坊
$hash = $wallet->send($to_address, 0.000000000000000001);
echo "发送以太坊 hash: $hash \n";
// 发送代币
$hash = $wallet->send($to_address, 1, $tm_contract, 8);
echo "发送代币 hash: $hash \n";

```
