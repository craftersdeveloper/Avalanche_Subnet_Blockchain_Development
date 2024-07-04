# Introduction To Avalanche Blockchain 

Avalanche is an open-source platform for launching decentralized applications (dApps) and enterprise blockchain deployments, known for its high throughput, low latency, and scalability. It can handle thousands of transactions per second with near-instant finality, making it one of the fastest smart contract platforms. Avalanche operates on a unique consensus protocol called Avalanche Consensus, which ensures robustness and security. It supports multiple custom blockchain networks and allows interoperability between them, making it versatile for various use cases, from finance to supply chain management.

## Avalanche Subnet

An Avalanche Subnet (short for subnetwork) is a dynamic set of validators working together to achieve consensus on the state of one or more blockchains within the Avalanche network. Subnets enable customizable, application-specific networks with tailored rulesets, allowing developers to create private or public networks with specific requirements such as compliance, security, and performance. Each Subnet can host multiple blockchains, and validators can participate in multiple Subnets, facilitating interoperability and enhancing the scalability and flexibility of the Avalanche ecosystem.

Avalanche's **Primary Network** is a special Subnet running three blockchains:
<br>
**The Platform Chain (P-Chain)
The Contract Chain (C-Chain)
The Exchange Chain (X-Chain)**

## Subnet Installation
The fastest way to install the latest Avalanche-CLI binary is by running the install script:

```shell
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
```

The binary installs inside the ~/bin directory. If the directory doesn't exist, it will be created.

You can run all of the commands in this tutorial by calling ~/bin/avalanche.

You can also add the command to your system path by running

```shell
export PATH=~/bin:$PATH
```
If you add it to your path, you should be able to call the program anywhere with just avalanche. To add it to your path permanently, add an export command to your shell initialization script (ex: .bashrc or .zshrc).

For more detailed installation instructions, see **Avalanche-CLI Installation**

## Contract 'Token' Explanation 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    // State variables
    uint public totalSupply; // Tracks the total supply of the token
    mapping(address => uint) public balanceOf; // Maps addresses to their token balances
    mapping(address => mapping(address => uint)) public allowance; // Maps owner addresses to spender addresses to allowed amounts

    string public name = "Solidity by Example"; // Name of the token
    string public symbol = "SOLBYEX"; // Symbol of the token
    uint8 public decimals = 18; // Number of decimals the token uses

    // Events
    event Transfer(address indexed from, address indexed to, uint value); // Emitted when tokens are transferred
    event Approval(address indexed owner, address indexed spender, uint value); // Emitted when an approval is made

    // Transfer function: Moves `amount` tokens from the caller's account to `recipient`
    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount; // Decreases the sender's balance by `amount`
        balanceOf[recipient] += amount; // Increases the recipient's balance by `amount`
        emit Transfer(msg.sender, recipient, amount); // Emits a Transfer event
        return true; // Indicates success
    }

    // Approve function: Allows `spender` to withdraw from the caller's account multiple times, up to `amount`
    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount; // Sets the allowance of `spender` to withdraw from the caller's account
        emit Approval(msg.sender, spender, amount); // Emits an Approval event
        return true; // Indicates success
    }

    // TransferFrom function: Moves `amount` tokens from `sender` to `recipient` using the allowance mechanism
    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount; // Decreases the allowance by `amount`
        balanceOf[sender] -= amount; // Decreases the sender's balance by `amount`
        balanceOf[recipient] += amount; // Increases the recipient's balance by `amount`
        emit Transfer(sender, recipient, amount); // Emits a Transfer event
        return true; // Indicates success
    }

    // Mint function: Creates `amount` new tokens and assigns them to the caller, increasing the total supply
    function mint(uint amount) external {
        balanceOf[msg.sender] += amount; // Increases the caller's balance by `amount`
        totalSupply += amount; // Increases the total supply by `amount`
        emit Transfer(address(0), msg.sender, amount); // Emits a Transfer event from the zero address
    }

    // Burn function: Destroys `amount` tokens from the caller, reducing the total supply
    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount; // Decreases the caller's balance by `amount`
        totalSupply -= amount; // Decreases the total supply by `amount`
        emit Transfer(msg.sender, address(0), amount); // Emits a Transfer event to the zero address
    }
}
```

### Explanation:

#### SPDX License Identifier
```solidity
// SPDX-License-Identifier: MIT
```
- Indicates that the code is licensed under the MIT License.

#### Pragma Directive
```solidity
pragma solidity ^0.8.17;
```
- Specifies the Solidity compiler version required to compile this contract.

#### State Variables
```solidity
uint public totalSupply;
```
- Tracks the total supply of the token.

```solidity
mapping(address => uint) public balanceOf;
```
- Maps addresses to their respective token balances.

```solidity
mapping(address => mapping(address => uint)) public allowance;
```
- Maps owner addresses to spender addresses to their respective allowances.

```solidity
string public name = "Solidity by Example";
string public symbol = "SOLBYEX";
uint8 public decimals = 18;
```
- These define the token's name, symbol, and decimal places.

#### Events
```solidity
event Transfer(address indexed from, address indexed to, uint value);
```
- Emitted when tokens are transferred.

```solidity
event Approval(address indexed owner, address indexed spender, uint value);
```
- Emitted when a token owner approves a spender.

#### Transfer Function
```solidity
function transfer(address recipient, uint amount) external returns (bool) {
    balanceOf[msg.sender] -= amount;
    balanceOf[recipient] += amount;
    emit Transfer(msg.sender, recipient, amount);
    return true;
}
```
- Moves `amount` tokens from the caller's account to `recipient`.

#### Approve Function
```solidity
function approve(address spender, uint amount) external returns (bool) {
    allowance[msg.sender][spender] = amount;
    emit Approval(msg.sender, spender, amount);
    return true;
}
```
- Allows `spender` to withdraw from the caller's account multiple times, up to `amount`.

#### TransferFrom Function
```solidity
function transferFrom(
    address sender,
    address recipient,
    uint amount
) external returns (bool) {
    allowance[sender][msg.sender] -= amount;
    balanceOf[sender] -= amount;
    balanceOf[recipient] += amount;
    emit Transfer(sender, recipient, amount);
    return true;
}
```
- Moves `amount` tokens from `sender` to `recipient` using the allowance mechanism.

#### Mint Function
```solidity
function mint(uint amount) external {
    balanceOf[msg.sender] += amount;
    totalSupply += amount;
    emit Transfer(address(0), msg.sender, amount);
}
```
- Creates `amount` new tokens and assigns them to the caller, increasing the total supply.

#### Burn Function
```solidity
function burn(uint amount) external {
    balanceOf[msg.sender] -= amount;
    totalSupply -= amount;
    emit Transfer(msg.sender, address(0), amount);
}
```
- Destroys `amount` tokens from the caller, reducing the total supply.

## Contract 'Vault' Explanation 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// Interface for the ERC20 standard
interface IERC20 {
    // Returns the total token supply
    function totalSupply() external view returns (uint);

    // Returns the account balance of another account with address `account`
    function balanceOf(address account) external view returns (uint);

    // Transfers `amount` tokens to address `recipient`
    function transfer(address recipient, uint amount) external returns (bool);

    // Returns the amount which `spender` is still allowed to withdraw from `owner`
    function allowance(address owner, address spender) external view returns (uint);

    // Allows `spender` to withdraw from your account multiple times, up to the `amount`
    function approve(address spender, uint amount) external returns (bool);

    // Transfers `amount` tokens from address `sender` to address `recipient` using the allowance mechanism
    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    // Event that is emitted when tokens are transferred
    event Transfer(address indexed from, address indexed to, uint value);

    // Event that is emitted when an approval is made
    event Approval(address indexed owner, address indexed spender, uint value);
}

// Vault contract
contract Vault {
    IERC20 public immutable token; // ERC20 token used by the vault

    uint public totalSupply; // Total supply of shares in the vault
    mapping(address => uint) public balanceOf; // Maps addresses to their share balances

    // Constructor: Initializes the vault with the ERC20 token address
    constructor(address _token) {
        token = IERC20(_token); // Sets the token as an ERC20 token
    }

    // Internal function to mint shares to an address
    function _mint(address _to, uint _shares) private {
        totalSupply += _shares; // Increases total supply by `_shares`
        balanceOf[_to] += _shares; // Increases balance of `_to` by `_shares`
    }

    // Internal function to burn shares from an address
    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares; // Decreases total supply by `_shares`
        balanceOf[_from] -= _shares; // Decreases balance of `_from` by `_shares`
    }

    // Deposits tokens into the vault and mints shares
    function deposit(uint _amount) external {
        /*
        a = amount
        B = balance of token before deposit
        T = total supply
        s = shares to mint

        (T + s) / T = (a + B) / B 

        s = aT / B
        */
        uint shares;
        if (totalSupply == 0) {
            shares = _amount; // If no shares exist, mint 1:1
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this)); // Calculate shares based on existing balance
        }

        _mint(msg.sender, shares); // Mint calculated shares to sender
        token.transferFrom(msg.sender, address(this), _amount); // Transfer tokens from sender to vault
    }

    // Withdraws tokens from the vault by burning shares
    function withdraw(uint _shares) external {
        /*
        a = amount
        B = balance of token before withdraw
        T = total supply
        s = shares to burn

        (T - s) / T = (B - a) / B 

        a = sB / T
        */
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply; // Calculate amount of tokens to withdraw
        _burn(msg.sender, _shares); // Burn the shares from sender
        token.transfer(msg.sender, amount); // Transfer the calculated amount of tokens to sender
    }
}
```

### Explanation:

#### SPDX License Identifier
```solidity
// SPDX-License-Identifier: MIT
```
- Indicates that the code is licensed under the MIT License.

#### Pragma Directive
```solidity
pragma solidity ^0.8.17;
```
- Specifies the Solidity compiler version required to compile this contract.

#### ERC20 Interface
```solidity
interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}
```
- Defines the standard functions and events for ERC20 tokens. These include functions for checking total supply, balance, transferring tokens, and approving allowances. Events for transfer and approval are also defined.

#### Vault Contract
```solidity
contract Vault {
    IERC20 public immutable token; // ERC20 token used by the vault
    uint public totalSupply; // Total supply of shares in the vault
    mapping(address => uint) public balanceOf; // Maps addresses to their share balances

    constructor(address _token) {
        token = IERC20(_token); // Sets the token as an ERC20 token
    }
```
- This contract manages deposits and withdrawals of a specific ERC20 token. It has state variables to track the token used, the total supply of shares, and the balance of shares for each user. The `constructor` initializes the vault with a specific ERC20 token address.

#### Mint Function
```solidity
    function _mint(address _to, uint _shares) private {
        totalSupply += _shares; // Increases total supply by `_shares`
        balanceOf[_to] += _shares; // Increases balance of `_to` by `_shares`
    }
```
- An internal function that increases the total supply of shares and the balance of the specified address by the given number of shares.

#### Burn Function
```solidity
    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares; // Decreases total supply by `_shares`
        balanceOf[_from] -= _shares; // Decreases balance of `_from` by `_shares`
    }
```
- An internal function that decreases the total supply of shares and the balance of the specified address by the given number of shares.

#### Deposit Function
```solidity
    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount; // If no shares exist, mint 1:1
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this)); // Calculate shares based on existing balance
        }

        _mint(msg.sender, shares); // Mint calculated shares to sender
        token.transferFrom(msg.sender, address(this), _amount); // Transfer tokens from sender to vault
    }
```
- Allows users to deposit a specified amount of tokens into the vault. The number of shares to mint is calculated based on the existing token balance in the vault. The tokens are transferred from the user to the vault, and the corresponding number of shares are minted for the user.

#### Withdraw Function
```solidity
    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply; // Calculate amount of tokens to withdraw
        _burn(msg.sender, _shares); // Burn the shares from sender
        token.transfer(msg.sender, amount); // Transfer the calculated amount of tokens to sender
    }
```
- Allows users to withdraw tokens from the vault by burning a specified number of shares. The amount of tokens to withdraw is calculated based on the share balance and the total supply of shares. The shares are burned from the user's balance, and the corresponding amount of tokens is transferred back to the user.



