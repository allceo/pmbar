sql分割字段中数据并显示成多条记录
=====================

比如:表T_Info中,存在２个字段　id(int),info(varchar600)

id         info
1001       110,111,112,113
1002       201,202,203
1003       110,201

将info字段中每个“逗号（，）”间的内容都分别提取出来，并显示成新列，且与“id”列关联
格式如下：

id       info
1001     110
1001     111
1001     112
1001     113
1002     201
1002     202
1002     203
1003     110
1003     201

----------

生成測試數據
  
    if not object_id('Tab') is null
        drop table Tab
    Go
    Create table Tab([Col1] int,[COl2] nvarchar(5))
    Insert Tab
    select 1,N'a,b,c' union all
    select 2,N'd,e' union all
    select 3,N'f'
    Go
 
SQL2000用辅助表:

    if object_id('Tempdb..#Num') is not null
        drop table #Num
    go
    select top 100 ID=Identity(int,1,1) into #Num from syscolumns a,syscolumns b
    Select 
        a.Col1,COl2=substring(a.Col2,b.ID,charindex(',',a.Col2+',',b.ID)-b.ID) 
    from 
        Tab a,#Num b
    where
        charindex(',',','+a.Col2,b.ID)=b.ID --也可用 substring(','+a.COl2,b.ID,1)=','
 
SQL2005用Xml:

    select 
        a.COl1,b.Col2
    from 
        (select Col1,COl2=convert(xml,'<root><v>'+replace(COl2,',','</v><v>')+'</v></root>') from Tab)a
    outer apply
        (select Col2=C.v.value('.','nvarchar(100)') from a.COl2.nodes('/root/v')C(v))b

SQL2005用CTE:
 

    ;with roy as 
    (select Col1,COl2=cast(left(Col2,charindex(',',Col2+',')-1) as nvarchar(100)),Split=cast(stuff(COl2+',',1,charindex(',',Col2+','),'') as nvarchar(100)) from Tab
    union all
    select Col1,COl2=cast(left(Split,charindex(',',Split)-1) as nvarchar(100)),Split= cast(stuff(Split,1,charindex(',',Split),'') as nvarchar(100)) from Roy where split>''
    )
    select COl1,COl2 from roy order by COl1 option (MAXRECURSION 0)

生成结果:
/*
Col1        COl2
----------- -----
1           a
1           b
1           c
2           d
2           e
3           f
*/