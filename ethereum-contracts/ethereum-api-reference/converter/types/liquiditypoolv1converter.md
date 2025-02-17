This contract is a specialized version of a converter that manages

a classic bancor liquidity pool.

Even though pools can have many reserves, the standard pool configuration

is 2 reserves with 50%/50% weights.

# Functions:

- [`constructor(contract IDSToken _token, contract IContractRegistry _registry, uint32 _maxConversionFee)`](#LiquidityPoolV1Converter-constructor-contract-IDSToken-contract-IContractRegistry-uint32-)

- [`converterType()`](#LiquidityPoolV1Converter-converterType--)

- [`acceptAnchorOwnership()`](#LiquidityPoolV1Converter-acceptAnchorOwnership--)

- [`addReserve(contract IReserveToken _token, uint32 _weight)`](#LiquidityPoolV1Converter-addReserve-contract-IReserveToken-uint32-)

- [`targetAmountAndFee(contract IReserveToken _sourceToken, contract IReserveToken _targetToken, uint256 _amount)`](#LiquidityPoolV1Converter-targetAmountAndFee-contract-IReserveToken-contract-IReserveToken-uint256-)

- [`recentAverageRate(contract IReserveToken _token)`](#LiquidityPoolV1Converter-recentAverageRate-contract-IReserveToken-)

- [`addLiquidity(contract IReserveToken[] _reserveTokens, uint256[] _reserveAmounts, uint256 _minReturn)`](#LiquidityPoolV1Converter-addLiquidity-contract-IReserveToken---uint256---uint256-)

- [`removeLiquidity(uint256 _amount, contract IReserveToken[] _reserveTokens, uint256[] _reserveMinReturnAmounts)`](#LiquidityPoolV1Converter-removeLiquidity-uint256-contract-IReserveToken---uint256---)

- [`fund(uint256 _amount)`](#LiquidityPoolV1Converter-fund-uint256-)

- [`liquidate(uint256 _amount)`](#LiquidityPoolV1Converter-liquidate-uint256-)

- [`addLiquidityCost(contract IReserveToken[] _reserveTokens, uint256 _reserveTokenIndex, uint256 _reserveAmount)`](#LiquidityPoolV1Converter-addLiquidityCost-contract-IReserveToken---uint256-uint256-)

- [`addLiquidityReturn(contract IReserveToken[] _reserveTokens, uint256[] _reserveAmounts)`](#LiquidityPoolV1Converter-addLiquidityReturn-contract-IReserveToken---uint256---)

- [`removeLiquidityReturn(uint256 _amount, contract IReserveToken[] _reserveTokens)`](#LiquidityPoolV1Converter-removeLiquidityReturn-uint256-contract-IReserveToken---)

## Function `constructor(contract IDSToken _token, contract IContractRegistry _registry, uint32 _maxConversionFee)` {#LiquidityPoolV1Converter-constructor-contract-IDSToken-contract-IContractRegistry-uint32-}

initializes a new LiquidityPoolV1Converter instance

## Parameters:

- `_token`:              pool token governed by the converter

- `_registry`:           address of a contract registry contract

- `_maxConversionFee`:   maximum conversion fee, represented in ppm

## Function `converterType() → uint16` {#LiquidityPoolV1Converter-converterType--}

returns the converter type

## Return Values:

- see the converter types in the the main contract doc

## Function `acceptAnchorOwnership()` {#LiquidityPoolV1Converter-acceptAnchorOwnership--}

accepts ownership of the anchor after an ownership transfer

also activates the converter

can only be called by the contract owner

note that prior to version 28, you should use 'acceptTokenOwnership' instead

## Function `addReserve(contract IReserveToken _token, uint32 _weight)` {#LiquidityPoolV1Converter-addReserve-contract-IReserveToken-uint32-}

defines a new reserve token for the converter

can only be called by the owner while the converter is inactive

## Parameters:

- `_token`:   address of the reserve token

- `_weight`:  reserve weight, represented in ppm, 1-1000000

## Function `targetAmountAndFee(contract IReserveToken _sourceToken, contract IReserveToken _targetToken, uint256 _amount) → uint256, uint256` {#LiquidityPoolV1Converter-targetAmountAndFee-contract-IReserveToken-contract-IReserveToken-uint256-}

returns the expected target amount of converting one reserve to another along with the fee

## Parameters:

- `_sourceToken`: contract address of the source reserve token

- `_targetToken`: contract address of the target reserve token

- `_amount`:      amount of tokens received from the user

## Return Values:

- expected target amount

- expected fee

## Function `recentAverageRate(contract IReserveToken _token) → uint256, uint256` {#LiquidityPoolV1Converter-recentAverageRate-contract-IReserveToken-}

returns the recent average rate of 1 `_token` in the other reserve token units

note that the rate can only be queried for reserves in a standard pool

## Parameters:

- `_token`:   token to get the rate for

## Return Values:

- recent average rate between the reserves (numerator)

- recent average rate between the reserves (denominator)

## Function `addLiquidity(contract IReserveToken[] _reserveTokens, uint256[] _reserveAmounts, uint256 _minReturn) → uint256` {#LiquidityPoolV1Converter-addLiquidity-contract-IReserveToken---uint256---uint256-}

increases the pool's liquidity and mints new shares in the pool to the caller

note that prior to version 28, you should use 'fund' instead

## Parameters:

- `_reserveTokens`:   address of each reserve token

- `_reserveAmounts`:  amount of each reserve token

- `_minReturn`:       token minimum return-amount

## Return Values:

- amount of pool tokens issued

## Function `removeLiquidity(uint256 _amount, contract IReserveToken[] _reserveTokens, uint256[] _reserveMinReturnAmounts) → uint256[]` {#LiquidityPoolV1Converter-removeLiquidity-uint256-contract-IReserveToken---uint256---}

decreases the pool's liquidity and burns the caller's shares in the pool

note that prior to version 28, you should use 'liquidate' instead

## Parameters:

- `_amount`:                  token amount

- `_reserveTokens`:           address of each reserve token

- `_reserveMinReturnAmounts`: minimum return-amount of each reserve token

## Return Values:

- the amount of each reserve token granted for the given amount of pool tokens

## Function `fund(uint256 _amount) → uint256` {#LiquidityPoolV1Converter-fund-uint256-}

increases the pool's liquidity and mints new shares in the pool to the caller

for example, if the caller increases the supply by 10%,

then it will cost an amount equal to 10% of each reserve token balance

note that starting from version 28, you should use 'addLiquidity' instead

## Parameters:

- `_amount`:  amount to increase the supply by (in the pool token)

## Return Values:

- amount of pool tokens issued

## Function `liquidate(uint256 _amount) → uint256[]` {#LiquidityPoolV1Converter-liquidate-uint256-}

decreases the pool's liquidity and burns the caller's shares in the pool

for example, if the holder sells 10% of the supply,

then they will receive 10% of each reserve token balance in return

note that starting from version 28, you should use 'removeLiquidity' instead

## Parameters:

- `_amount`:  amount to liquidate (in the pool token)

## Return Values:

- the amount of each reserve token granted for the given amount of pool tokens

## Function `addLiquidityCost(contract IReserveToken[] _reserveTokens, uint256 _reserveTokenIndex, uint256 _reserveAmount) → uint256[]` {#LiquidityPoolV1Converter-addLiquidityCost-contract-IReserveToken---uint256-uint256-}

given the amount of one of the reserve tokens to add liquidity of,

returns the required amount of each one of the other reserve tokens

since an empty pool can be funded with any list of non-zero input amounts,

this function assumes that the pool is not empty (has already been funded)

## Parameters:

- `_reserveTokens`:       address of each reserve token

- `_reserveTokenIndex`:   index of the relevant reserve token

- `_reserveAmount`:       amount of the relevant reserve token

## Return Values:

- the required amount of each one of the reserve tokens

## Function `addLiquidityReturn(contract IReserveToken[] _reserveTokens, uint256[] _reserveAmounts) → uint256` {#LiquidityPoolV1Converter-addLiquidityReturn-contract-IReserveToken---uint256---}

returns the amount of pool tokens entitled for given amounts of reserve tokens

since an empty pool can be funded with any list of non-zero input amounts,

this function assumes that the pool is not empty (has already been funded)

## Parameters:

- `_reserveTokens`:   address of each reserve token

- `_reserveAmounts`:  amount of each reserve token

## Return Values:

- the amount of pool tokens entitled for the given amounts of reserve tokens

## Function `removeLiquidityReturn(uint256 _amount, contract IReserveToken[] _reserveTokens) → uint256[]` {#LiquidityPoolV1Converter-removeLiquidityReturn-uint256-contract-IReserveToken---}

returns the amount of each reserve token entitled for a given amount of pool tokens

## Parameters:

- `_amount`:          amount of pool tokens

- `_reserveTokens`:   address of each reserve token

## Return Values:

- the amount of each reserve token entitled for the given amount of pool tokens
