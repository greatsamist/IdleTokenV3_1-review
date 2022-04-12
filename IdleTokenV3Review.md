# IDLE TOKEN V3 CONTRACT REVIEW

## What is Idle (IDLE) | What is IDLE token

### What is Idle </br>

Idle is a decentralized Defi protocol dedicated to bringing automated asset allocation and aggregation to the interest-bearing tokens economy. IdleTokens allow users to optimize profitability and seamlessly get the highest yield, without having to manually switch funds between lending protocols.
</br>

```js
import "@openzeppelin/contracts-ethereum-package/contracts/math/SafeMath.sol";
```

**SafeMath** is used to against overflow and underflow issue in solidity

```js
import "@openzeppelin/contracts-ethereum-package/contracts/token/ERC20/SafeERC20.sol";
```

**SafeERC20** is a wrapper around the interface that eliminates the need to handle boolean return values. used for securely interacting with existing tokens that might fail.

```js
import "@openzeppelin/contracts-ethereum-package/contracts/token/ERC20/ERC20.sol";
```

OZ token standard implementation.

```js
import "@openzeppelin/contracts-ethereum-package/contracts/lifecycle/Pausable.sol";
```

**Pausable** allows children to implement an emergency stop mechanism that can be triggered by an authorized account..
This has event **Paused** and **Unpaused** emitted when triggered by a pauser.

```js
import "@openzeppelin/upgrades/contracts/Initializable.sol";
```

The contract is using OpenZeppelin Upgrades. In this case a proxy contract as already been deployed as the storage layer. This V3 will be deployed and the Proxy is updated to reference the new V3 address.

```js
import "./interfaces/iERC20Fulcrum.sol";
```

This contract is inheriting The Fulcrun Token contract

# Function Definition

## Function initialize

**Regular Function is used instead of a constructor**

**_IdleTokenV3_1.initialize(name, symbol, decimals) _**

> Details: Sets the values for `name`, `symbol`, and `decimals`. All three of these values are immutable: they can only be set once during construction.

**_IdleTokenV3_1.initialize(msg.sender) _**

> Details: Initializes the contract in unpaused state. Assigns the Pauser role to the deployer.

**_IdleTokenV3_1.initialize(\_name, \_symbol, \_token, \_iToken, \_cToken, \_rebalancer) _**

> Details: constructor, initialize some variables, mainly addresses of other contracts

## _function_ owner

**_IdleTokenV3_1.owner() constant,view_**

> Details: Returns the address of the current owner.

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | address  |                 |

## _function_ pause

**_IdleTokenV3_1.pause() _**

> Details: Called by a pauser to pause, triggers stopped state.

## _function_ paused

**_IdleTokenV3_1.paused() constant,view_**

> Details: Returns true if the contract is paused, and false otherwise.

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | bool     |                 |

## _function_ protocolWrappers

**_IdleTokenV3_1.protocolWrappers() constant,view_**

Arguments

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | address  |                 |

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | address  |                 |

## _function_ rebalance

**_IdleTokenV3_1.rebalance() _**

> Notice: Dynamic allocate all the pool across different lending protocols if needed, rebalance without params \* NOTE: this method can be paused

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | bool     |                 |

## _function_ rebalanceWithGST

**_IdleTokenV3_1.rebalanceWithGST() _**

> Notice: Dynamic allocate all the pool across different lending protocols if needed, use gas refund from gasToken \* NOTE: this method can be paused. msg.sender should approve this contract to spend GST2 tokens before calling this method

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | bool     |                 |

## _function_ rebalancer

**_IdleTokenV3_1.rebalancer() constant,view_**

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | address  |                 |

## _function_ redeemIdleToken

**_IdleTokenV3_1.redeemIdleToken(\_amount) _**

> Notice: Here we calc the pool share one can withdraw given the amount of IdleToken they want to burn NOTE: If the contract is paused or iToken price has decreased one can still redeem but no rebalance happens. NOTE 2: If iToken price has decresed one should not redeem (but can do it) otherwise he would capitalize the loss. Ideally one should wait until the black swan event is terminated

Arguments

| **name** | **type** | **description**                     |
| -------- | -------- | ----------------------------------- |
| \_amount | uint256  | : amount of IdleTokens to be burned |

Outputs

| **name**       | **type** | **description** |
| -------------- | -------- | --------------- |
| redeemedTokens | uint256  |                 |

## _function_ redeemInterestBearingTokens

**_IdleTokenV3_1.redeemInterestBearingTokens(\_amount) _**

> Notice: Here we calc the pool share one can withdraw given the amount of IdleToken they want to burn and send interest-bearing tokens (eg. cDAI/iDAI) directly to the user. Underlying (eg. DAI) is not redeemed here.

Arguments

| **name** | **type** | **description**                     |
| -------- | -------- | ----------------------------------- |
| \_amount | uint256  | : amount of IdleTokens to be burned |

## _function_ renounceOwnership

**_IdleTokenV3_1.renounceOwnership() _**

> Details: Leaves the contract without owner. It will not be possible to call `onlyOwner` functions anymore. Can only be called by the current owner. \* > Note: Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

## _function_ renouncePauser

**_IdleTokenV3_1.renouncePauser() _**

## _function_ setAllAvailableTokensAndWrappers

**_IdleTokenV3_1.setAllAvailableTokensAndWrappers(protocolTokens, wrappers) _**

> Notice: It allows owner to modify allAvailableTokens array in case of emergency ie if a bug on a interest bearing token is discovered and reset protocolWrappers associated with those tokens.

Arguments

| **name**       | **type**  | **description**                                                    |
| -------------- | --------- | ------------------------------------------------------------------ |
| protocolTokens | address[] | : array of protocolTokens addresses (eg [cDAI, iDAI, ...])         |
| wrappers       | address[] | : array of wrapper addresses (eg [IdleCompound, IdleFulcrum, ...]) |

## _function_ setFee

**_IdleTokenV3_1.setFee(\_fee) _**

> Notice: It allows owner to set the fee (1000 == 10% of gained interest)

Arguments

| **name** | **type** | **description**                                        |
| -------- | -------- | ------------------------------------------------------ |
| \_fee    | uint256  | : fee amount where 100000 is 100%, max settable is 10% |

## _function_ setFeeAddress

**_IdleTokenV3_1.setFeeAddress(\_feeAddress) _**

> Notice: It allows owner to set the fee address

Arguments

| **name**     | **type** | **description** |
| ------------ | -------- | --------------- |
| \_feeAddress | address  | : fee address   |

## _function_ setGovTokens

**_IdleTokenV3_1.setGovTokens(\_newGovTokens) _**

> Notice: It allows owner to set gov tokens array In case of any errors gov distribution can be paused by passing an empty array

Arguments

| **name**       | **type**  | **description**                       |
| -------------- | --------- | ------------------------------------- |
| \_newGovTokens | address[] | : array of governance token addresses |

## _function_ setIToken

**_IdleTokenV3_1.setIToken(\_iToken) _**

> Notice: It allows owner to set the \_iToken address

Arguments

| **name** | **type** | **description**                                                 |
| -------- | -------- | --------------------------------------------------------------- |
| \_iToken | address  | : new \_iToken address (can be address(0) which means disabled) |

## _function_ setMaxUnlentPerc

**_IdleTokenV3_1.setMaxUnlentPerc(\_perc) _**

> Notice: It allows owner to set the max unlent asset percentage (1000 == 1% of unlent asset max)

Arguments

| **name** | **type** | **description**                        |
| -------- | -------- | -------------------------------------- |
| \_perc   | uint256  | : max unlent perc where 100000 is 100% |

## _function_ setRebalancer

**_IdleTokenV3_1.setRebalancer(\_rebalancer) _**

> Notice: It allows owner to set the IdleRebalancerV3_1 address

Arguments

| **name**     | **type** | **description**                  |
| ------------ | -------- | -------------------------------- |
| \_rebalancer | address  | : new IdleRebalancerV3_1 address |

## _function_ symbol

**_IdleTokenV3_1.symbol() constant,view_**

> Details: Returns the symbol of the token, usually a shorter version of the name.

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | string   |                 |

## _function_ token

**_IdleTokenV3_1.token() constant,view_**

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | address  |                 |

## _function_ tokenPrice

**_IdleTokenV3_1.tokenPrice() constant,view_**

> Notice: IdleToken price calculation, in underlying

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | uint256  |                 |

## _function_ totalSupply

**_IdleTokenV3_1.totalSupply() constant,view_**

> Details: See {IERC20-totalSupply}.

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | uint256  |                 |

## _function_ transfer

**_IdleTokenV3_1.transfer(recipient, amount) _**

> Notice: ERC20 modified transfer that also update the avgPrice paid for the recipient and updates user gov idx

Arguments

| **name**  | **type** | **description**     |
| --------- | -------- | ------------------- |
| recipient | address  | : recipient account |
| amount    | uint256  | : value to transfer |

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | bool     |                 |

## _function_ transferFrom

**_IdleTokenV3_1.transferFrom(sender, recipient, amount) _**

> Notice: ERC20 modified transferFrom that also update the avgPrice paid for the recipient and updates user gov idx

Arguments

| **name**  | **type** | **description**     |
| --------- | -------- | ------------------- |
| sender    | address  | : sender account    |
| recipient | address  | : recipient account |
| amount    | uint256  | : value to transfer |

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | bool     |                 |

## _function_ transferOwnership

**_IdleTokenV3_1.transferOwnership(newOwner) _**

> Details: Transfers ownership of the contract to a new account (`newOwner`). Can only be called by the current owner.

Arguments

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
| newOwner | address  |                 |

## _function_ unpause

**_IdleTokenV3_1.unpause() _**

> Details: Called by a pauser to unpause, returns to normal state.

## _function_ userAvgPrices

**_IdleTokenV3_1.userAvgPrices() constant,view_**

Arguments

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | address  |                 |

Outputs

| **name** | **type** | **description** |
| -------- | -------- | --------------- |
|          | uint256  |                 |

## _function_ usersGovTokensIndexes

**_IdleTokenV3_1.usersGovTokensIndexes(, ) constant,view_**
