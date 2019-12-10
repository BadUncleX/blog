---
layout: post
title:  " dapp概述以及Solidity"
date:   2018-07-27 19:03:59 +0800
categories: dapp solidity 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
深入浅出ETH原理与智能合约课程笔记


* TOC
{:toc}




## dapp用例

![](/assets/images/2018-07/2018-07-27-111743.jpg)



![](/assets/images/2018-07/2018-07-27-111818.jpg)



查看dApp的网站：

https://www.stateofthedapps.com/ 

https://dappradar.com/ 





![](/assets/images/2018-07/2018-07-27-112117.jpg)





## 合约结构

![](/assets/images/2018-07/2018-07-27-112532.jpg)



在线Solidity编辑 IDE https://remix.ethereum.org/



ERC20规范

![](/assets/images/2018-07/2018-07-27-120832.jpg)

ERC20本质： 两张表， 第一张表是记录所有账户余额， 第二张表是记录两步操作， 主要涉及智能合约里的transferFrom（有一个预先额度分配， 预付费-获取报酬）



ERC721



![](/assets/images/2018-07/2018-07-27-122247.jpg)

> 代币是不可拆分的

METADATA： URI， 比如猫的图片



721规范地址： https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md 







## Solidity语法



solidity简介

![](/assets/images/2018-07/2018-07-27-123026.jpg)

![](/assets/images/2018-07/2018-07-27-124234.jpg)



![](/assets/images/2018-07/2018-07-27-124638.jpg)



代码示例

```solo
pragma solidity ^0.4.0;
contract Ballot {

    struct Voter {
        uint weight;
        bool voted;
        uint8 vote;
        address delegate;
    }
    struct Proposal {
        uint voteCount;
    }

    address chairperson;
    mapping(address => Voter) voters;
    
    ///动态数组
    Proposal[] proposals;

    /// Create a new ballot with $(_numProposals) different proposals.
    /// 构造函数
    function Ballot(uint8 _numProposals) public {
        chairperson = msg.sender;
        voters[chairperson].weight = 1;
        proposals.length = _numProposals;
    }

    /// Give $(toVoter) the right to vote on this ballot.
    /// May only be called by $(chairperson).
    function giveRightToVote(address toVoter) public {
        if (msg.sender != chairperson || voters[toVoter].voted) return;
        voters[toVoter].weight = 1;
    }

    /// Delegate your vote to the voter $(to).
    function delegate(address to) public {
        Voter storage sender = voters[msg.sender]; // assigns reference
        if (sender.voted) return;
        while (voters[to].delegate != address(0) && voters[to].delegate != msg.sender)
            to = voters[to].delegate;
        if (to == msg.sender) return;
        sender.voted = true;
        sender.delegate = to;
        Voter storage delegateTo = voters[to];
        if (delegateTo.voted)
            proposals[delegateTo.vote].voteCount += sender.weight;
        else
            delegateTo.weight += sender.weight;
    }

    /// Give a single vote to proposal $(toProposal).
    function vote(uint8 toProposal) public {
        Voter storage sender = voters[msg.sender];
        if (sender.voted || toProposal >= proposals.length) return;
        sender.voted = true;
        sender.vote = toProposal;
        proposals[toProposal].voteCount += sender.weight;
    }

    function winningProposal() public constant returns (uint8 _winningProposal) {
        uint256 winningVoteCount = 0;
        for (uint8 prop = 0; prop < proposals.length; prop++)
            if (proposals[prop].voteCount > winningVoteCount) {
                winningVoteCount = proposals[prop].voteCount;
                _winningProposal = prop;
            }
    }
}
```



Remix在线调试合约



系统提供5个帐户， ca3作为chairman， giveRightto 147、4b0 

ca3 deploy 3

ca3 vote 1

147 vote 2

4b0 vote 1

winningProposal 1



