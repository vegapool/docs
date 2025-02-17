This contract is the main entry point for Bancor token conversions.

It also allows for the conversion of any token in the Bancor Network to any other token in a single

transaction by providing a conversion path.

A note on Conversion Path: Conversion path is a data structure that is used when converting a token

to another token in the Bancor Network, when the conversion cannot necessarily be done by a single

converter and might require multiple 'hops'.

The path defines which converters should be used and what kind of conversion should be done in each step.

The path format doesn't include complex structure; instead, it is represented by a single array

in which each 'hop' is represented by a 2-tuple - converter anchor & target token.

In addition, the first element is always the source token.

The converter anchor is only used as a pointer to a converter (since converter addresses are more

likely to change as opposed to anchor addresses).

Format:

[source token, converter anchor, target token, converter anchor, target token...]

# Functions:

- [`constructor(contract IContractRegistry _registry)`](#BancorNetwork-constructor-contract-IContractRegistry-)

- [`conversionPath(contract IReserveToken _sourceToken, contract IReserveToken _targetToken)`](#BancorNetwork-conversionPath-contract-IReserveToken-contract-IReserveToken-)

- [`rateByPath(address[] _path, uint256 _amount)`](#BancorNetwork-rateByPath-address---uint256-)

- [`convertByPath2(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary)`](#BancorNetwork-convertByPath2-address---uint256-uint256-address-payable-)

- [`xConvert(address[] _path, uint256 _amount, uint256 _minReturn, bytes32 _targetBlockchain, bytes32 _targetAccount, uint256 _conversionId)`](#BancorNetwork-xConvert-address---uint256-uint256-bytes32-bytes32-uint256-)

- [`completeXConversion(address[] _path, contract IBancorX _bancorX, uint256 _conversionId, uint256 _minReturn, address payable _beneficiary)`](#BancorNetwork-completeXConversion-address---contract-IBancorX-uint256-uint256-address-payable-)

- [`getReturnByPath(address[] _path, uint256 _amount)`](#BancorNetwork-getReturnByPath-address---uint256-)

- [`convertByPath(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary, address, uint256)`](#BancorNetwork-convertByPath-address---uint256-uint256-address-payable-address-uint256-)

- [`convert(address[] _path, uint256 _amount, uint256 _minReturn)`](#BancorNetwork-convert-address---uint256-uint256-)

- [`convert2(address[] _path, uint256 _amount, uint256 _minReturn, address, uint256)`](#BancorNetwork-convert2-address---uint256-uint256-address-uint256-)

- [`convertFor(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary)`](#BancorNetwork-convertFor-address---uint256-uint256-address-payable-)

- [`convertFor2(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary, address, uint256)`](#BancorNetwork-convertFor2-address---uint256-uint256-address-payable-address-uint256-)

- [`claimAndConvert(address[] _path, uint256 _amount, uint256 _minReturn)`](#BancorNetwork-claimAndConvert-address---uint256-uint256-)

- [`claimAndConvert2(address[] _path, uint256 _amount, uint256 _minReturn, address, uint256)`](#BancorNetwork-claimAndConvert2-address---uint256-uint256-address-uint256-)

- [`claimAndConvertFor(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary)`](#BancorNetwork-claimAndConvertFor-address---uint256-uint256-address-payable-)

- [`claimAndConvertFor2(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary, address, uint256)`](#BancorNetwork-claimAndConvertFor2-address---uint256-uint256-address-payable-address-uint256-)

# Events:

- [`Conversion(contract IConverterAnchor _smartToken, contract IReserveToken _fromToken, contract IReserveToken _toToken, uint256 _fromAmount, uint256 _toAmount, address _trader)`](#BancorNetwork-Conversion-contract-IConverterAnchor-contract-IReserveToken-contract-IReserveToken-uint256-uint256-address-)

## Function `constructor(contract IContractRegistry _registry)` {#BancorNetwork-constructor-contract-IContractRegistry-}

initializes a new BancorNetwork instance

## Parameters:

- `_registry`:    address of a contract registry contract

## Function `conversionPath(contract IReserveToken _sourceToken, contract IReserveToken _targetToken) → address[]` {#BancorNetwork-conversionPath-contract-IReserveToken-contract-IReserveToken-}

returns the conversion path between two tokens in the network

note that this method is quite expensive in terms of gas and should generally be called off-chain

## Parameters:

- `_sourceToken`: source reserve token address

- `_targetToken`: target reserve token address

## Return Values:

- conversion path between the two tokens

## Function `rateByPath(address[] _path, uint256 _amount) → uint256` {#BancorNetwork-rateByPath-address---uint256-}

returns the expected target amount of converting a given amount on a given path

note that there is no support for circular paths

## Parameters:

- `_path`:        conversion path (see conversion path format above)

- `_amount`:      amount of _path[0] tokens received from the sender

## Return Values:

- expected target amount

## Function `convertByPath2(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary) → uint256` {#BancorNetwork-convertByPath2-address---uint256-uint256-address-payable-}

converts the token to any other token in the bancor network by following

a predefined conversion path and transfers the result tokens to a target account

note that the network should already have been given allowance of the source token (if not ETH)

## Parameters:

- `_path`:                conversion path, see conversion path format above

- `_amount`:              amount to convert from, in the source token

- `_minReturn`:           if the conversion results in an amount smaller than the minimum return - it is cancelled, must be greater than zero

- `_beneficiary`:         account that will receive the conversion result or 0x0 to send the result to the sender account

## Return Values:

- amount of tokens received from the conversion

## Function `xConvert(address[] _path, uint256 _amount, uint256 _minReturn, bytes32 _targetBlockchain, bytes32 _targetAccount, uint256 _conversionId) → uint256` {#BancorNetwork-xConvert-address---uint256-uint256-bytes32-bytes32-uint256-}

converts any other token to BNT in the bancor network by following

a predefined conversion path and transfers the result to an account on a different blockchain

note that the network should already have been given allowance of the source token (if not ETH)

## Parameters:

- `_path`:                conversion path, see conversion path format above

- `_amount`:              amount to convert from, in the source token

- `_minReturn`:           if the conversion results in an amount smaller than the minimum return - it is cancelled, must be greater than zero

- `_targetBlockchain`:    blockchain BNT will be issued on

- `_targetAccount`:       address/account on the target blockchain to send the BNT to

- `_conversionId`:        pre-determined unique (if non zero) id which refers to this transaction

## Return Values:

- the amount of BNT received from this conversion

## Function `completeXConversion(address[] _path, contract IBancorX _bancorX, uint256 _conversionId, uint256 _minReturn, address payable _beneficiary) → uint256` {#BancorNetwork-completeXConversion-address---contract-IBancorX-uint256-uint256-address-payable-}

allows a user to convert a token that was sent from another blockchain into any other

token on the BancorNetwork

ideally this transaction is created before the previous conversion is even complete, so

so the input amount isn't known at that point - the amount is actually take from the

BancorX contract directly by specifying the conversion id

## Parameters:

- `_path`:            conversion path

- `_bancorX`:         address of the BancorX contract for the source token

- `_conversionId`:    pre-determined unique (if non zero) id which refers to this conversion

- `_minReturn`:       if the conversion results in an amount smaller than the minimum return - it is cancelled, must be nonzero

- `_beneficiary`:     wallet to receive the conversion result

## Return Values:

- amount of tokens received from the conversion

## Function `getReturnByPath(address[] _path, uint256 _amount) → uint256, uint256` {#BancorNetwork-getReturnByPath-address---uint256-}

deprecated, backward compatibility

## Function `convertByPath(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary, address, uint256) → uint256` {#BancorNetwork-convertByPath-address---uint256-uint256-address-payable-address-uint256-}

deprecated, backward compatibility

## Function `convert(address[] _path, uint256 _amount, uint256 _minReturn) → uint256` {#BancorNetwork-convert-address---uint256-uint256-}

deprecated, backward compatibility

## Function `convert2(address[] _path, uint256 _amount, uint256 _minReturn, address, uint256) → uint256` {#BancorNetwork-convert2-address---uint256-uint256-address-uint256-}

deprecated, backward compatibility

## Function `convertFor(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary) → uint256` {#BancorNetwork-convertFor-address---uint256-uint256-address-payable-}

deprecated, backward compatibility

## Function `convertFor2(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary, address, uint256) → uint256` {#BancorNetwork-convertFor2-address---uint256-uint256-address-payable-address-uint256-}

deprecated, backward compatibility

## Function `claimAndConvert(address[] _path, uint256 _amount, uint256 _minReturn) → uint256` {#BancorNetwork-claimAndConvert-address---uint256-uint256-}

deprecated, backward compatibility

## Function `claimAndConvert2(address[] _path, uint256 _amount, uint256 _minReturn, address, uint256) → uint256` {#BancorNetwork-claimAndConvert2-address---uint256-uint256-address-uint256-}

deprecated, backward compatibility

## Function `claimAndConvertFor(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary) → uint256` {#BancorNetwork-claimAndConvertFor-address---uint256-uint256-address-payable-}

deprecated, backward compatibility

## Function `claimAndConvertFor2(address[] _path, uint256 _amount, uint256 _minReturn, address payable _beneficiary, address, uint256) → uint256` {#BancorNetwork-claimAndConvertFor2-address---uint256-uint256-address-payable-address-uint256-}

deprecated, backward compatibility

## Event `Conversion(contract IConverterAnchor _smartToken, contract IReserveToken _fromToken, contract IReserveToken _toToken, uint256 _fromAmount, uint256 _toAmount, address _trader)` {#BancorNetwork-Conversion-contract-IConverterAnchor-contract-IReserveToken-contract-IReserveToken-uint256-uint256-address-}

triggered when a conversion between two tokens occurs

## Parameters:

- `_smartToken`:  anchor governed by the converter

- `_fromToken`:   source reserve token

- `_toToken`:     target reserve token

- `_fromAmount`:  amount converted, in the source token

- `_toAmount`:    amount returned, minus conversion fee

- `_trader`:      wallet that initiated the trade
