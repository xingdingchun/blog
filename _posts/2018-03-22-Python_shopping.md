---
layout: post
title: Python 实现简单购物程序
key: 20180322
tags: Python
---

## 命令行下用Python实现简单购物程序

> 1. 循环的理解,多层循环理解。
>
> 2. 判断理解，多层判断理解。
>
> 3. 列表使用。



<pre>
#!/usr/bin/env python
#-*- coding:utf-8 -*-
# __Authour__:Chadwick.Xing
# __Date__:2018/3/22

# 程序需求：使用Python写一个在命令行执行的购物车程序
# 要求：1，让用户输入想消费的金额(能消费的金额)
#       2，显示(打印)所有商品供用户选择
#       3，如果用户余额不足，提示用户，并显示所有商品
#       4，退出是显示用户所购买的所有商品和剩余金额

# 定义商品列表
goods_menu = [
    ['Iphone X',8500],
    ['Python book',80],
    ['bike',2000],
    ['Mac book',10000],
    ['杯子',25],
    ['戴尔台式电脑',5000],
    ['办公桌子',7000],
]

# 定义接收已购买商品的列表
shopping_cart = []

# 输入金额，如果不是数字，允许尝试三次，三次过后退出
input_num = 0
while input_num < 3:
    consumption = input("Please input your consumption amount >>>: ")
    if consumption.isdigit():
        consumption = int(consumption)
        break
    else:
        print("Please input correct money.")
        input_num += 1
else:
    print("Your input too many number.")

while True:
    # 打印所有商品列表
    for number,menu in enumerate(goods_menu,1):
        print("No.",number," Goods names: ",menu[0],"Goods price: ",menu[1])
    # 提示用户输入需要选择商品的序号
    choice_goods = input("Please input your choice goods [1/2/3/.../quit] >>>: ")
    # 判断用户输入的是否是数字
    if choice_goods.isdigit():
        # 输入的是数字，则修改为int类型
        choice_goods = int(choice_goods)
        # 判断输入的序号是否在商品范围内
        if choice_goods > 0 and choice_goods <= len(goods_menu):
            # 序号显示是从1开始的，列表下标是从0 开始的需要减1
            num_choice = goods_menu[choice_goods - 1]
            # 判断消费的钱是否可以购买这个商品
            if consumption >= num_choice[1]:
                # 商品加入购物车
                shopping_cart.append(num_choice[0])
                # 减去相应的消费的费用
                consumption -= num_choice[1]
            else:
                # 提示用户消费的金额不足以购买此商品
                print("Your is amount not insufficient.")
        else:
            # 提示用户输入的内容不正确或者不在菜单范围内
            print("Your choice goods not exist.")
    # 如果输入的quit显示相关内容，并退出程序
    elif choice_goods == "quit":
        print("----------------您购买的商品列表------------")
        # 显示所购买的商品列表
        for v,j in enumerate(shopping_cart):
            print(v,j)
        # 显示余额
        print("Your is consumption amount: ",consumption)
        # 退出循环
        break
    # 提示输入的内容不存在
    else:
        print("Your is choice goods not exist.")
else:
    # 提示用购物结束
    print('I wish you a happy life at the end of the shopping.')
</pre>