// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Context.sol";

contract MyToken is ERC20, Ownable {
    mapping(address => bool) private blacklist;
    mapping(address => bool) _blacklist;
    event BlacklistUpdated(address indexed user, bool value);
    
    constructor() ERC20("KBToken", "KB") {
        _mint(msg.sender, 10000000 * 10 ** uint(decimals()));
    }

    function burn(uint256 amount) public onlyOwner{
        _burn(_msgSender(), amount);
    }

    function burnFrom(address account, uint256 amount) public onlyOwner {
        _spendAllowance(account, _msgSender(), amount);
        _burn(account, amount);
    }

    function blacklistUpdate(address user, bool value) public virtual onlyOwner {
        _blacklist[user] = value;
        emit BlacklistUpdated(user, value);
    }

    function isBlackListed(address user) public view returns (bool) {
        return _blacklist[user];
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual override(ERC20) {
        require (!isBlackListed(to), "Token transfer refused. Receiver is on blacklist");
        require (!isBlackListed(from), "Token transfer refused. You are on blacklist");
        super._beforeTokenTransfer(from, to, amount);
    }
}


