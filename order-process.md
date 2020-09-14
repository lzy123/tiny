# 下单流程
## 描述
下单流程涉及到寄送地址、商品信息、商品促销、礼券使用、商品库存扣减等等问题
- 获取购物车商品信息
- 查询商品对应的库存
  - 注意库存的幂等性，即重复提交数据不一致
  - 注意库存的数据的一致性，高并发下，操作的对象一样，未作排他处理，最终更新导致数据非一致性
  - 解决上述的方式，采用设置库存，而非直接扣减更新库存，且通过版本号或旧库存新旧数据判断,伪代码：<br />
    transaction  
    
    select stock as old_stock,  [或者 version]  from stock_table where sid=sid
    
    update  stock_table set stock = new_stock where sid=sid and stock=old_stock
    
    或
    
    update stock_table set version = version+1, stock = new_stock where sid=sid and version=old_version
    
    commit
- 获取商品促销信息
- 获取订单邮寄费用
- 计算最终的订单的支付金额


