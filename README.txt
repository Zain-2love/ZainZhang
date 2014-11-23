关于sql 两表关联查询的语句问题  和 理解<br/>
如果你涉及到了两表关联查询  那么一定会写基本的 查询语句
sql: select * from A
一个简单的查询表A 所有字段的语句
那么接下来我们开始两表关联的语句
sql: select * from A left join A.id = B.a_id
或者
sql: select * from A left join B ON A.id = B.a_id left join C ON C.b_id = B.id 
一个新的关键字  left join 它就是关联 两张表的语句 和它有关的还有 right join  和 inner join  者三个分别是 一个左关联 和右关联 最后一个是并不以谁为准 只查询符合条件的
好吧 初学者到这里已经搞不明白了 
那我们一起去理解它 
下面是我在工作中遇到的一个四张表关联的sql 语句 你不用看多仔细
SELECT
product.officeName,
product.customerName,
product.productType,
repairwork.problemDes,
taskmessage.contacts,
repairwork.repairWorkId,
product.enginnerName,
taskmessage.tel,
(select time_format(timediff(NOW(), repairwork.arriveDateTime),'%H') ) AS time
from TaskMessage left  JOIN bankorganization
ON taskmessage.bankOrgId = bankorganization.bankOrganizationId
LEFT JOIN product
ON product.bankOrgId = bankorganization.bankOrganizationId
LEFT JOIN repairwork
ON repairwork.taskMessage_taskMessageId = taskmessage.taskMessageId
where 
(select time_format(timediff(NOW(), repairwork.arriveDateTime),'%H') )>1 and TaskMessage.resolveType=1 and RepairWork.workStatus=2

我们来简化一下它
首先用 * 来显示出它所能查询到的所有字段
SELECT * from TaskMessage left  JOIN bankorganization
ON taskmessage.bankOrgId = bankorganization.bankOrganizationId
LEFT JOIN product
ON product.bankOrgId = bankorganization.bankOrganizationId
LEFT JOIN repairwork
ON repairwork.taskMessage_taskMessageId = taskmessage.taskMessageId
where 
(select time_format(timediff(NOW(), repairwork.arriveDateTime),'%H') )>1 and TaskMessage.resolveType=1 and RepairWork.workStatus=2
再去掉where 条件语句
SELECT * from TaskMessage left  JOIN bankorganization
ON taskmessage.bankOrgId = bankorganization.bankOrganizationId
LEFT JOIN product
ON product.bankOrgId = bankorganization.bankOrganizationId
LEFT JOIN repairwork
ON repairwork.taskMessage_taskMessageId = taskmessage.taskMessageId;
再简化一个它的关联
SELECT * from A left  JOIN B
ON A.b_id = B.id
LEFT JOIN C
ON C.b_id = B.id
LEFT JOIN D
ON D.a_id = A.Id;
好了 这样就写成了一个四张表A,B,C,D关联的sql 语句了

这时你可能会说  可我还是不会关联啊 我不知道什么字段和什么字段关联啊
不要着急 
关键是理解到视图的存在 视图是什么呢 就是
sql: select * from A
这句话所运行出来后出现的东西叫视图
它不可以修改 不可以删除
只是临时用来查看的
回原来的话题  如何找处关联字段?
首先我们把上面那段简化后的sql 拿来说示例
SELECT * from A 
LEFT JOIN B ON A.b_id = B.id
LEFT JOIN C ON C.b_id = B.id
LEFT JOIN D ON D.a_id = A.Id;
我们先运行第一行
得到表A的视图
在视图里得到一个字段叫 b_id 
然后我 LEFT JOIN B ON A.b_id = B.id 把A表和B表关联起来
得出第二行
然后我们第一行和第二行一起运行  得出两表关联的所有字段
之后两句sql 依次运行
然后我们就写完了 就这样多表关联就这样













