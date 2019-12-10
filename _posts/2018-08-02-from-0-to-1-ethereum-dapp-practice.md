---
layout: post
title:  " 从0到1构建基于以太坊智能合约的ICO DApp 实战篇 "
date:   2018-08-02  +0800
categories: 以太坊 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
from 从0到 1构建基于以太坊智能合约的 ICO DApp


* TOC
{:toc}






如果需要搞清楚任何一个信息系统，核心关注点只有两个，系统的数据结构和状态流转。数据结构通常可以理解为数据库表结构、系统中关键实体的属性，而状态流转常常和业务流程有关，即如何操作数据、数据如何变化，对应到实际的开发中就是系统对外暴露的接口，接口内部的业务规则。



Project.sol

```js
pragma solidity ^0.4.24;

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    function mul(uint a, uint b) internal pure returns (uint) {
        uint c = a * b;
        assert(a == 0 || c / a == b);
        return c;
    }

    function div(uint a, uint b) internal pure returns (uint) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function sub(uint a, uint b) internal pure returns (uint) {
        assert(b <= a);
        return a - b;
    }

    function add(uint a, uint b) internal pure returns (uint) {
        uint c = a + b;
        assert(c >= a);
        return c;
    }
}

contract ProjectList {
    using SafeMath for uint;
    address[] public projects;

    function createProject(string _description, uint _minInvest, uint _maxInvest, uint _goal) public {
        address newProject = new Project(_description, _minInvest, _maxInvest, _goal, msg.sender);
        projects.push(newProject);
    }

    function getProjects() public view returns(address[]) {
        return projects;
    }
}

contract Project {
    using SafeMath for uint;

    struct Payment {
        string description;
        uint amount;
        address receiver;
        bool completed;
        mapping(address => bool) voters;
        uint voterCount;
    }

    address public owner;
    string public description;
    uint public minInvest;
    uint public maxInvest;
    uint public goal;
    uint public investorCount;
    mapping(address => uint) public investors;

    Payment[] public payments;

    modifier ownerOnly() {
        require(msg.sender == owner);
        _;
    }

    constructor(string _description, uint _minInvest, uint _maxInvest, uint _goal, address _owner) public {
        require(_maxInvest >= _minInvest);
        require(_goal >= _minInvest);
        require(_goal >= _maxInvest);

        description = _description;
        minInvest = _minInvest;
        maxInvest = _maxInvest;
        goal = _goal;
        owner = _owner;
    }

    function contribute() public payable {
        require(msg.value >= minInvest);
        require(msg.value <= maxInvest);

        uint newBalance = 0;
        newBalance = address(this).balance.add(msg.value);
        require(newBalance <= goal);

        if (investors[msg.sender] > 0) {
            investors[msg.sender] += msg.value;
        } else {
            investors[msg.sender] = msg.value;
            investorCount += 1;
        }
    }

    function createPayment(string _description, uint _amount, address _receiver) ownerOnly public {
        Payment memory newPayment = Payment({
            description: _description,
            amount: _amount,
            receiver: _receiver,
            completed: false,
            voterCount: 0
        });

        payments.push(newPayment);
    }

    function approvePayment(uint index) public {
        Payment storage payment = payments[index];

        // must be investor to vote
        require(investors[msg.sender] > 0);

        // can not vote twice
        require(!payment.voters[msg.sender]);

        payment.voters[msg.sender] = true;
        payment.voterCount += 1;
    }

    function doPayment(uint index) ownerOnly public {
        Payment storage payment = payments[index];

        require(!payment.completed);
        require(address(this).balance >= payment.amount);
        require(payment.voterCount > (investorCount / 2));

        payment.receiver.transfer(payment.amount);
        payment.completed = true;
    }

    function getSummary() public view returns (string, uint, uint, uint, uint, uint, uint, address) {
        return (
            description,
            minInvest,
            maxInvest,
            goal,
            address(this).balance,
            investorCount,
            payments.length,
            owner
        );
    }
}
```





solidity会报的一个错误

```bash
ParserError: Expected pragma, import directive or contract/interface/library definition.
```

原因， 第一行的pragma没有分号。





Remix在Chrome和Firefox上表现不一样， 貌似Firefox上正常点。 



碰到循环语句gas不能评估的报错， 改为非循环的以后仍然有错， 明天继续。 



