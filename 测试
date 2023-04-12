# -*- coding: utf-8 -*-
import threading
import requests
import sqlite3
import time
import json
import random
import os
import smtplib
from email.mime.text import MIMEText
from email.header import Header
import random
session = requests.Session()


def pdip():
    conn = sqlite3.connect('jiage.db')
    cursor = conn.cursor()
    # 向pdip表中写入数据
    cursor.execute('DELETE FROM pdip')
    cursor.execute("INSERT INTO pdip (ip) VALUES ('1')")
    conn.commit()
    conn.close()


def dl_ip():
    # 声明proxies变量为全局变量
    global proxies
    connection = sqlite3.connect('ips.db')
    cursor = connection.cursor()
    # 查询所有IP
    cursor.execute("SELECT ip FROM ip_table")
    ips = cursor.fetchall()
    # 随机选择一个IP
    random_ip = random.choice(ips)[0].strip()
    # 打印IP
    #print(random_ip)
    # 去除'\r'字符
    random_ip = random_ip.replace('\r', '')
    # 修改全局变量proxies
    proxies = {"http": "http://" + random_ip, "https": "http://" + random_ip}
    # 打印代理字典
    #print(proxies)
    # 关闭数据库连接
    return proxies
    connection.close()

def xdip():
    # 声明proxies变量为全局变量
    global xdproxies
    connection = sqlite3.connect('xdip.db')
    cursor = connection.cursor()
    # 查询所有IP
    cursor.execute("SELECT ip FROM ip_table")
    xdips = cursor.fetchall()
    # 随机选择一个IP
    random_ip = random.choice(xdips)[0].strip()
    # 打印IP
    #print(random_ip)
    # 去除'\r'字符
    random_ip = random_ip.replace('\r', '')
    # 修改全局变量proxies
    xdproxies = {"http": "http://" + random_ip, "https": "http://" + random_ip}
    # 打印代理字典
    #print(proxies)
    # 关闭数据库连接
    return xdproxies
    connection.close()



while True:
    data_list = []

    # 循环从1到3
    for page in range(1, 2):
        data1 = {
            'castingId': '1725',
            'page': '1',
            'pageSize': '15',
            'sort': '2',
            'transactionStatus': '2'
        }
        extracted_info = {
            # 这里需要填写每次登录的token
            'token': 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiIxODg4OTU4NjEyOCIsInNvdXJjZSI6InBjIiwidHlwZSI6ImN1c3RvbWVyIiwiZXhwIjoxNjgxNzg4MTgzLCJzaWduSWQiOiJiYjBhY2IxZWU3NTk0NTk4YTdkMWI1NWFhZmMwMzlkMiIsImlhdCI6MTY4MTE4MzM4M30.Gb0HgZSNrCZ0oLo5mr4mYaUEbYRTUlE9cG4LfQSG8Ps'
        }
        # 将data的内容添加到data_list列表
        data_list.append(data1)
        while True:
            try:
                proxies = dl_ip()
                gm_url = 'https://api2.gandart.com/market/api/v2/resaleManage/resale/onSale'
                gm = requests.post(url=gm_url, headers=extracted_info, data=data1, proxies=proxies)
                gmnr = gm.json()  # 将响应的 JSON 格式数据解析为 Python 对象
                print('已经在获取商品价格！！！')
                print(gmnr)
                # print(data)
                # gm_url = 'https://app-api.mayi.art/api/market/market/getMarketGoodsListByGoodsId'
                # gm = session.post(url=gm_url, headers=extracted_info, data=data1,proxies=proxies)
                # gmnr1 = gm.text
                # print(gmnr1)
                # print('gmnr1')
                # gmnr = json.loads(gmnr1)
                # print(dl_json)
                # gmnr= gm.json()   # 将响应的 JSON 格式数据解析为 Python 对象
                print('已经在获取商品价格！！！')
                if '太火爆了，请稍后再试' in gmnr['msg'] or "操作太频繁" in gmnr["msg"] or "访问频繁，请10分钟后再试" in \
                        gmnr["msg"]:  # 如果响应消息中包含 "太火爆了，请稍后再试" 或 "操作太频繁"
                    raise Exception('太火爆了，请稍后再试')  # 抛出一个异常
                    print('有异常！！')
                print('没有异常！！！')
                break  # 如果没有异常，
            except Exception as e:  # 如果出现异常
                # print(gmnr)
                # print('aaaaa')
                print(e)  # 打印异常信息
                # 如果提示 太火爆了，请稍后再试和操作太频繁，就等待5秒后运行
                time.sleep(3)  # 等待 5 秒钟
        data_list.append(gmnr)  # 将响应的 JSON 格式数据添加到列表 data_lis

        print('执行这了吗')
        for item in gmnr['obj']['list']:  # 遍历 gmnr 响应数据中的 data 属性下的 list 属性中的元素
            if item['transactionStatus'] == 2 and float(item['resalePrice']) < 151.00:  # 如果元素中的 status 属性等于 1 且 price 属性小于 42
                gm_id = ('{0}'.format(item['id']))  # 获取元素中的 id 属性的值
                print('正在提交订单！！！')
                gm1_url = 'https://api.gandart.com/base/v2/resaleManage/resale/buy/v2'  # 定义一个 URL
                gm1_data = {  # 定义一个字典类型的数据对象 gm1_data
                    'transactionRecordId': gm_id
                }
                print('提交中！！！')

                xdproxies = xdip()

                gm_1 = requests.post(url=gm1_url, headers=extracted_info, data=gm1_data,proxies=xdproxies)  # 发送 POST 请求到指定 URL，使用指定 headers 和 data，使用指定的代理服务器 proxies
                gm_11 = gm_1.text
                gmnrr = json.loads(gm_11)  # 将响应的 JSON 格式数据解析为 Python 对象
                print(gmnrr)


                gmnr = gmnrr
                while True:
                    if gmnr['success'] == True:
                        print('下单成功！！')

                        # 发送邮件的邮箱和密码
                        sender = 'm18889586128@163.com'
                        password = 'OSSSAHLGWMMIAAIO'

                        # 接收邮件的邮箱
                        receiver = '2443162397@qq.com'

                        # 邮件主题和正文内容
                        subject = '大哥,大哥,抢到了！！！'
                        success = gmnr['success']  # 下单状态
                        price = gmnr['obj']['price']  # 下单价格
                        orderNum = gmnr['obj']['orderNum']  # 订单号
                        collectionName = gmnr['obj']['collectionName']  # 商品名称

                        mail_content = f"商品名称: {collectionName}\n下单价格: {price}元！！\n订单号: {orderNum}"

                        # 构造邮件
                        msg = MIMEText(mail_content, 'plain', 'utf-8')
                        msg['Subject'] = Header(subject, 'utf-8')
                        msg['From'] = Header('温馨提醒', 'utf-8')
                        msg['To'] = Header(receiver, 'utf-8')

                        # 使用smtplib发送邮件
                        try:
                            # 连接邮箱服务器
                            server = smtplib.SMTP_SSL('smtp.163.com', 465)
                            # 登录邮箱
                            server.login(sender, password)
                            # 发送邮件
                            server.sendmail(sender, [receiver], msg.as_string())
                            print("邮件发送成功")
                        except Exception as e:
                            print("邮件发送失败")
                        finally:
                            # 关闭连接
                            server.quit()
                        # print('你好1')
                        break  # 跳出循环
                    elif  gmnr['success'] == False or '存在未付款的订单!' in gmnr["msg"]:
                        #print('1')
                        print(gmnr["msg"])
                        break  # 跳出循环
                    break  # 跳出循环
                    # print('bbbb')
            if gmnr['success'] == False or '存在未付款的订单!' in gmnr["msg"]:  # 如果响应信息中包含"下单成功"或"存在其他尚未支付的订单"，则跳出while循环。
                pdip()
                #print('2')
                print(gmnr["msg"])
            # print('你好2')
                break  # 跳出循环


                # 这段代码是用来处理商品下单的响应数据的。
                # 首先将响应数据转换为json格式，并将转换后的结果保存在变量gmnrr中。
                # 接着，使用一个while循环来判断是否下单成功或者存在未支付的订单。
                # 如果下单成功，就打印出抢到的价格，并使用break语句退出循环；
                # 如果存在未支付的订单，就打印出提示信息，并使用break语句退出循环。
                # 注意，如果响应数据中的msg字段既不包含"下单成功"，也不包含"存在其他尚未支付的订单"，
                # 那么也会使用break语句退出循环。
            else:
                print("正在拼命抢购！！！！")  # 如果元素中的没有满足 status 属性等于 1 且 price 属性小于 42  就会运行这里的代码
        if gmnr['success'] == False or '存在未付款的订单!' in gmnr["msg"]:  # 如果响应信息中包含"下单成功"或"存在其他尚未支付的订单"，则跳出while循环。
            print(gmnr["msg"])
            pdip()
            # print('你好3')
            #print('3')
            break  # 跳出循环
        # time.sleep(7)  # 等待2秒钟刷新下一个页面
    print('啊啊啊啊啊！！！！')
    # 添加退出循环的条件
    if gmnr['success'] == False or '存在未付款的订单!' in gmnr["msg"]:
        print(gmnr["msg"])
        pdip()
        #print('4')
        # print('你好4')
        break  # 跳出循环
    # 运行结束后等待两秒钟从新运行
    # time.sleep(1)

