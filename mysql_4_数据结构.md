# 减少数据冗余
有点数据可以有其他列计算得到。

#尽量避免数据出现更新，插入和删除异常。


# 节约空间 加快查询 

# 数据库结构设计的步骤
    1.需求分析
    了解产品设计的存储需求
    存储需求
    数据处理需求
    数据的安全性的完整性
    
    2.逻辑设计
    设计数据的逻辑存储结构
    数据实体之间的逻辑关系，解决数据冗余
    和数据维护
    
    3.根据使用的数据库特点进行表结构设计
    关系型
    
# 数据库设计范式 （三范式）
       第一范式：
       1.数据库表中的所有字段都具有单一属性
       2.单一属性的列是由基本的数据类型构成 
       3.二维表
      
       第二范式：
       一个表中具有一个业务主键。

       第三范式
       每一个非主键既不部分依赖也不传递依赖于业务主键。
        
# 日期的选择 
        timestamp  存时间戳  随时区变化而变化 多个字段更新时间，只能更新一个
        datetime   不随时间变化而变化
    
