---
layout: default
title: "我的股票"
tags: log
---


# 通达信公式
- 公式名称
  - 活跃股池
- 公式描述
  - 当天收盘执行，第二天开盘看盘选股使用
- 公式内容
{换手率}
HSL := (100*VOL/CAPITAL) >= 3 AND (100*VOL/CAPITAL) <= 20;  

{量比}
LB := DYNAINFO(17) >= 1.5 AND  DYNAINFO(17) <= 10; 

{A股且非创业非科创非ST}
MARKET := NOT(NAMELIKE('ST') OR NAMELIKE('*ST') OR NAMELIKE('S')
OR INBLOCK('创业板') OR INBLOCK('科创板'));

{趋势条件}
P1 := MA(CLOSE, 20);    {20日均线，用于判断最近1个月趋势}
P2 := MA(CLOSE, 60);    {60日均线，用于判断最近3个月趋势}

三个月趋势向上 := P2 > REF(P2, 1);  {60日均线逐日上升}
一个月趋势向上 := P1 > REF(P1, 1);  {20日均线逐日上升}

最近三个月涨幅 := (CLOSE / REF(CLOSE, 60) - 1) * 100; {最近3个月涨幅}
最近一个月涨幅 := (CLOSE / REF(CLOSE, 20) - 1) * 100; {最近1个月涨幅}

{最终筛选条件}
HSL AND LB AND MARKET AND 
三个月趋势向上 AND 一个月趋势向上 AND 
最近三个月涨幅 > 10 AND 最近一个月涨幅 > 5;


- 公式名称
  - 尾盘抢筹
- 公式描述
  - 当天早盘结束执行，午盘开盘选股使用
- 公式内容
VAR1:=(OPEN-REF(C,1))/REF(C,1)*100< 3;
VAR2:=(C-REF(C,1))/REF(C,1)*100< 5;
VAR3:=(C-REF(C,1))/REF(C,1)*100>2;
VAR4:=(REF(C,1)-REF(C,2))/REF(C,2)*100< 5;
VAR5:=(C-REF(C,29))/REF(C,29)*100< 60;
VAR6:=(H-C)/REF(C,1)*100< 4.3;
VAR9:=VOL/CAPITAL*100>5 AND VOL/CAPITAL*100< 10 AND V/REF(MA(V,5),1)>2;
VOLUME:=VOL;
MAVOL1:=MA(VOLUME,120);
VAR7:=VOL>MAVOL1*1.2;
去ST:=NOT(NAMELIKE('ST') OR NAMELIKE('*ST') OR NAMELIKE('S'));
VAR78:=(REF(HHV(C,12),1)-REF(LLV(C,12),1))/REF(LLV(C,12),1)*100< 18;
尾盘抢筹:VAR1 AND VAR2 AND VAR3 AND VAR4 AND VAR5 AND VAR6 AND VAR7 AND VAR78 AND 去ST AND VAR9;

# 通达信使用
- 盘中报警功能
- 个股训练模式



# stock
- 仓位
  - 牛100% 熊20% 横20~50%
- 选股/买入
  - 选股时间
    - 每晚
  - 买入时间
    - 开早盘一小时以内
    - 开午盘一小时以内
  - 筛选条件1
    - 活跃股+热门板块
  - 筛选条件2
    - 趋势
      - 上涨的 && 破位的
    - 形态
  - 筛选条件3
    - 量价关系
    - 指标
      - MA|MACD|KDJ|BOLL|RSI
  - 筛选条件4(确定有资金)
    - 三板定强庄
- 风控/卖出
  - 卖出时间
    - 开早盘一小时以内
    - 止赢止损单
    - 报警
  - 止损
    - 建仓后设置止损点(1.5*ATR) | 浮盈则动态上调止损位
  - 止赢
    - K线差/动能弱(5~20)|k线反弹(10~MAX[])
  - 其他出场方案
    - 时间维度
      - 7日后未触发止赢或止损
    - 行情不确定
      - 短线三卖
        - 开盘一小时不红盘
        - 盘中冲高回落
        - 盘中炸板
  - 卖出方式
    - 分批卖出/全出
- 执行
  - 无计划不买卖
  - 盘中不思考
  - 每日复盘
- 其他

## 看盘
- 自选类型
  - 版面-实时看股
    - 自定义的版面 成分股为前一天的选股
- 大盘类型
  - 股票行情-实时看盘/盘中监测/个股联动
- 查看美股纳指ETF
  - 市场 - 基金理财 - 开放式基金板块 - 纳指100指数基金 
  - 市场 - 基金理财 - 交易所基金 - 跨境ETF和QDII
