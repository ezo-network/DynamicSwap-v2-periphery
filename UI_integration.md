# Integration into UI

## 1. Circuit breakers

Allowed token price (rate to other token in pair) decrease per transaction and per day (or other time frame) you can get from pair contract calling function [vars(uint id)](https://github.com/ezo-network/bSwap-v2-core/blob/65ccc1eedb97d05ef4d961fc4a4b17f652e190ec/contracts/UniswapV2Pair.sol#L16-L25) 
where `id`:
0. timeFrame = 1 days;  // during this time frame rate of reserve1/reserve0 should be in range [baseLinePrice0*(1-maxDump0), baseLinePrice0*(1+maxDump1)]
1. maxDump0 = 100;    // maximum allowed dump (in percentage) of reserve1/reserve0 rate during time frame relatively the baseline
2. maxDump1 = 100;    // maximum allowed dump (in percentage) of reserve0/reserve1 rate during time frame relatively the baseline
3. maxTxDump0 = 100;  // maximum allowed dump (in percentage) of token0 price per transaction
4. maxTxDump1 = 100;  // maximum allowed dump (in percentage) of token1 price per transaction
5. coefficient = 100; // coefficient (in percentage) to transform price growing into fee. I.e if coefficient = 50% than if price growth buy 1% the fee will be 0.5% (price growth * coefficient).
6. minimalFee = 1;    // Minimal fee percentage (with 1 decimals) applied to transaction. I.e. 1 = 0.1%
7. periodMA = 45*60;  // MA period in seconds

## 2. Fluctuation fees

You have to get `amountOut` with 0% fee than get real `amountOut` using function [getAmountOut(uint amountIn, address tokenIn, address tokenOut)](https://github.com/ezo-network/bSwap-v2-core/blob/65ccc1eedb97d05ef4d961fc4a4b17f652e190ec/contracts/UniswapV2Pair.sol#L96) in the appropriate pair contract. 
`fee % = (amountOut without fee * 100) / amountOut with fee`

To get `amountOut` without fee, use following formula:

`amountOut = amountIn * reserveOut / (reserveIn + amountIn)`

where: `reserveIn, reserveOut` reserve from pair [getReserves()](https://github.com/ezo-network/bSwap-v2-core/blob/65ccc1eedb97d05ef4d961fc4a4b17f652e190ec/contracts/UniswapV2Pair.sol#L54) and sorted accordingly `tokenIn` and `tokenOut`.

## 3. Anti bot protection

When user sends swap transaction set `amountOutMin` parameter of swap functions (like [])

## 4. Business corelation


## 5. Standard uniswap functionality

BSwap implement all standard interfaces of Uniswap core and Router 2 (except functions: `getAmountOut(uint amountIn, uint reserveIn, uint reserveOut)` and `getAmountIn(uint amountOut, uint reserveIn, uint reserveOut)`. Instead of them should be used functions `getAmountsOut(uint amountIn, address[] memory path)` and `getAmountsIn(uint amountOut, address[] memory path)` accordingly).

