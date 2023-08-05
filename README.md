```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract GAYToken is IBEP20 {
    string public name = "GAY Token";
    string public symbol = "GAY";
    uint8 public decimals = 18;
    
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;
    
    uint256 private totalTokens = 1696969696 * 10**18; // Total Supply 
    
    constructor() {
        balances[msg.sender] = totalTokens;
        emit Transfer(address(0), msg.sender, totalTokens);
    }
    
    function totalSupply() external view override returns (uint256) {
        return totalTokens;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return balances[account];
    }
    
    function transfer(address recipient, uint256 amount) external override returns (bool) {
        require(amount <= balances[msg.sender], "Insufficient balance");

        balances[msg.sender] -= amount;
        balances[recipient] += amount;
        
        emit Transfer(msg.sender, recipient, amount);
        
        return true;
    }
    
    function allowance(address owner, address spender) external view override returns (uint256) {
        return allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) external override returns (bool) {
        allowances[msg.sender][spender] = amount;
        
        emit Approval(msg.sender, spender, amount);
        
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        require(amount <= balances[sender], "Insufficient balance");
        require(amount <= allowances[sender][msg.sender], "Insufficient allowance");

        balances[sender] -= amount;
        balances[recipient] += amount;
        allowances[sender][msg.sender] -= amount;
        
        emit Transfer(sender, recipient, amount);
        
        return true;
    }
}
```
