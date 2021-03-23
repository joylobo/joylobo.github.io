---
title: Leet-code(力扣)题解
date: 2021-03-03 14:22:40
tags:
    - 算法
---


# 1. 两数之和
<!-- 2021/03/03 -->
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。  
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。  
你可以按任意顺序返回答案。  

**示例 1:**
```
输入:nums = [2,7,11,15], target = 9
输出:[0,1]
解释:因为 nums[0] + nums[1] == 9 ，返回 [0, 1]
```
**示例 2:**
```
输入:nums = [3,2,4], target = 6
输出:[1,2]
```
**示例 3:**
```
输入:nums = [3,3], target = 6
输出:[0,1]
```
**提示:**
* 2 <= nums.length <= 103
* -109 <= nums[i] <= 109
* -109 <= target <= 109
* 只会存在一个有效答案

**解答:**
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    const MAP = new Map();
    MAP.set(nums[0], 0);

    for (let i = 1; i < nums.length; i++) {
        let other = target - nums[i];
        if (MAP.get(other) !== undefined) return [MAP.get(other), i];
        MAP.set(nums[i], i)
    }
};
```

# 2. 两数相加
<!-- 2021/03/03 -->
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。  
请你将两个数相加，并以相同形式返回一个表示和的链表。  
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。  

**示例 1:**

![示例 1](./leet-code/addtwonumber1.jpg)

```
输入:l1 = [2,4,3], l2 = [5,6,4]
输出:[7,0,8]
解释:342 + 465 = 807.
```
**示例 2:**
```
输入:l1 = [0], l2 = [0]
输出:[0]
```
**示例 3:**
```
输入:l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出:[8,9,9,9,0,0,0,1]
```
**提示:**
* 每个链表中的节点数在范围 [1, 100] 内
* 0 <= Node.val <= 9
* 题目数据保证列表表示的数字不含前导零

**解答:**
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let head = new ListNode(0)
    let tmp = 0
    let pos = head
    while (l1 || l2 || tmp !== 0) {
        const sum = ((l1 ? l1.val : 0) + (l2 ? l2.val : 0) + tmp)
        tmp = Math.floor(sum / 10)
        pos.next = new ListNode(sum % 10)
        pos = pos.next
        if (l1) l1 = l1.next
        if (l2) l2 = l2.next
    }

    return head.next
};
```
# 3. 无重复字符的最长子串
<!-- 2021/03/03 -->
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例 1:**
```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
**示例 2:**
```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
**示例 3:**
```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
**示例 4:**
```
输入: s = ""
输出: 0
```
**提示:**
* 0 <= s.length <= 5 * 104
* s 由英文字母、数字、符号和空格组成

**解答:**
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let kvStore = new Map()
    let cur = 0
    let max = 0
    
    const arr = s.split('')
    for (let i = 0; i < arr.length; i++) {
        if (kvStore.get(arr[i]) >= cur) {
            cur = kvStore.get(arr[i]) + 1
        } 
        
        max = i - cur + 1> max ? i - cur + 1 : max
        kvStore.set(arr[i], i)
    }

    return max
};
```