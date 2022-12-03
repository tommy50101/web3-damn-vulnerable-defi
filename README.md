![](cover.png)

**A set of challenges to learn offensive security of smart contracts in Ethereum.**

Featuring flash loans, price oracles, governance, NFTs, lending pools, smart contract wallets, timelocks, and more!

## Play

Visit [damnvulnerabledefi.xyz](https://damnvulnerabledefi.xyz)

## Disclaimer

All Solidity code, practices and patterns in this repository are DAMN VULNERABLE and for educational purposes only.

DO NOT USE IN PRODUCTION.

## 思路

### unstopable

UnstoppableLender.sol => 閃電貸借款池子

ReceiverUnstoppable.sol => 借款人自己的合約，所借款項會先轉到這裡面進行操作後再還回去池子


破口：

assert(poolBalance == balanceBefore);

思路就是，想辦法讓這個assert失效


觀察到他的 poolBalance 只有在呼叫 depositTokens(uint256 amount) 時，裏面加上 amount 才會增加

而 balanceBefore 的計算，則是從 token 合約中直接查詢池子地址所擁有的token數量


如果大家都乖乖透過 depositTokens() 去增加池子中 token，合約就沒問題

而我們只要 “不透過 depositTokens()” 去把 token 轉進池子合約中，poolBalance 就不會加上 amount 所以不會更新數量

poolBalance == balanceBefore 這段等式也就永遠不會成立，導致合約永遠無法使用了
 