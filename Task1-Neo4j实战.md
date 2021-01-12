# 准备工作

![image-20210111212033161](/Users/mac/Library/Application Support/typora-user-images/image-20210111212033161.png)

​		红色箭头指向的就是命令的输入框



1. 首先，我们删除数据库中以往的图，确保一个空白的环境进行操作：

   ```
   MATCH (n) DETACH DELETE n
   ```

   上面的命令中，`MATCH`是**匹配**操作，而小括号()代表一个**节点**node（可理解为括号类似一个圆形），括号里面的n为**标识符**。



# 2. Neo4j实战

## 2.1 创建节点

1. 可以一次只创建一个人物节点

   ```
   CREATE (n:Person {name:'Ray'}) RETURN n
   ```

   效果如下图，可以看到我们创建了一个**`标签`**为Person（也就是节点的类型），只具有一个**`name`**属性的节点，**`name`**属性值为Ray：

   ![image-20210111214424206](/Users/mac/Library/Application Support/typora-user-images/image-20210111214424206.png)

   点击红框中的”Graph“，可以看到图形化的节点：

   ![image-20210111214446247](/Users/mac/Library/Application Support/typora-user-images/image-20210111214446247.png)

2. 也可以一次创建多个人物节点

   ```
   CREATE (n:Person {name:'Sally'}) RETURN n;
   CREATE (n:Person {name:'Steve'}) RETURN n;
   CREATE (n:Person {name:'Mike'}) RETURN n;
   CREATE (n:Person {name:'Liz'}) RETURN n;
   CREATE (n:Person {name:'Shawn'}) RETURN n;
   ```

   > 注意：在每条命令后加上`;`即可一次运行多条命令。

   将多条命令放入命令框中，如下图：

   ![image-20210111214541769](/Users/mac/Library/Application Support/typora-user-images/image-20210111214541769.png)

   运行结束后，点击下图红框中的`Node Labels`即可显示图形化节点：

   ![image-20210111214727795](/Users/mac/Library/Application Support/typora-user-images/image-20210111214727795.png)

   ![image-20210111214748134](/Users/mac/Library/Application Support/typora-user-images/image-20210111214748134.png)

3. 接下来，通过以下命令创建地区节点：

   ```
   CREATE (n:Location {city:'Miami', state:'FL'});
   CREATE (n:Location {city:'Boston', state:'MA'});
   CREATE (n:Location {city:'Lynn', state:'MA'});
   CREATE (n:Location {city:'Portland', state:'ME'});
   CREATE (n:Location {city:'San Francisco', state:'CA'});
   ```

   可以看上，以上命令创建了5个节点，其均为`Location`类型，且节点属性包括`city`和`state`两种。如下图：

   ![image-20210111215145587](/Users/mac/Library/Application Support/typora-user-images/image-20210111215145587.png)



## 2.2 创建关系

### 2.2.1 已有节点之间的关系

#### 2.2.1.1朋友关系

1. 创建朋友关系（无属性）

   我们先为两个节点创建朋友关系：

   ```
   MATCH (a:Person {name:'Liz'}), 
         (b:Person {name:'Mike'}) 
   MERGE (a)-[:FRIENDS]->(b)
   ```

   ![image-20210111223626277](/Users/mac/Library/Application Support/typora-user-images/image-20210111223626277.png)

   方括号[]即为关系，FRIENDS为关系的类型。
   注意这里的箭头-->是有方向的，表示是从a到b的关系。 这样，Liz和Mike之间建立了FRIENDS关系。

2. 创建含有属性的关系

   ```
   MATCH (a:Person {name:'Shawn'}), 
         (b:Person {name:'Sally'}) 
   MERGE (a)-[:FRIENDS {since:2001}]->(b)
   ```

   如下图：

   ![image-20210111223846708](/Users/mac/Library/Application Support/typora-user-images/image-20210111223846708.png)

3. 增加多个含有since属性的朋友关系：

   ```
   MATCH (a:Person {name:'Shawn'}), (b:Person {name:'Ray'}) MERGE (a)-[:FRIENDS {since:2012}]->(b);
   MATCH (a:Person {name:'Mike'}), (b:Person {name:'Shawn'}) MERGE (a)-[:FRIENDS {since:2006}]->(b);
   MATCH (a:Person {name:'Sally'}), (b:Person {name:'Steve'}) MERGE (a)-[:MARRIED {since:2006}]->(b);
   MATCH (a:Person {name:'Liz'}), (b:Person {name:'Ray'}) MERGE (a)-[:FRIENDS {since:2007}]->(b);
   ```

4. 查看现在的节点情况：

   ![image-20210111224850417](/Users/mac/Library/Application Support/typora-user-images/image-20210111224850417.png)

#### 2.2.1.2 出生地关系

1. 建立不同类型节点之间的关系：人物和地点的关系

   ```
   MATCH (a:Person {name:'Ray'}), (b:Location {city:'Boston'}) MERGE (a)-[:BORN_IN {year:1998}]->(b);
   MATCH (a:Person {name:'Liz'}), (b:Location {city:'Boston'}) MERGE (a)-[:BORN_IN {year:1981}]->(b);
   MATCH (a:Person {name:'Mike'}), (b:Location {city:'San Francisco'}) MERGE (a)-[:BORN_IN {year:1960}]->(b);
   MATCH (a:Person {name:'Shawn'}), (b:Location {city:'Miami'}) MERGE (a)-[:BORN_IN {year:1960}]->(b);
   MATCH (a:Person {name:'Steve'}), (b:Location {city:'Lynn'}) MERGE (a)-[:BORN_IN {year:1970}]->(b);
   MATCH (a:Person {name:'Sally'}), (b:Location {city:'Portland'}) MERGE (a)-[:BORN_IN {year:1969}]->(b)
   ```

   MATCH表示匹配的是a,b两个节点，MERGE表示在两个节点之间添加一种关系。

   命令运行结果如下图：

   ![image-20210111225555337](/Users/mac/Library/Application Support/typora-user-images/image-20210111225555337.png)

### 2.2.2 同时创建节点和关系

1. 若想在创建节点的时候就建好关系：

   ```
   CREATE (a:Person {name:'Todd'})-[r:FRIENDS]->(b:Person {name:'Carlos'})
   ```

   如下图：

   ![image-20210111230321280](/Users/mac/Library/Application Support/typora-user-images/image-20210111230321280.png)



## 2.3 图数据库查询

1. 查询下所有在Boston出生的人物：

   ```
   MATCH (a:Person)-[:BORN_IN]->(b:Location {city:'Boston'}) RETURN a,b
   ```

   使用MATCH命令，Person节点不指定属性，但需要指定关系以及Location节点的属性。箭头无方向。

   ![image-20210111231006810](/Users/mac/Library/Application Support/typora-user-images/image-20210111231006810.png)

2. 查询所有**对外**有关系的节点

   ```
   MATCH (a)-->() RETURN a
   ```

   > 注意：这里箭头是有方向的，返回结果不含任何地区节点，因为地区并没有指向其他节点（只是被指向）

   ![image-20210111233904873](/Users/mac/Library/Application Support/typora-user-images/image-20210111233904873.png)

3. 查询所有有关系的节点

   ```
   MATCH (a)--() RETURN a
   ```

   > 注意，这里的箭头没有方向

   查询结果：

   ![image-20210111234031902](/Users/mac/Library/Application Support/typora-user-images/image-20210111234031902.png)

4. 查询所有对外有关系的节点，以及关系类型

   ```
   MATCH (a)-[r]->() RETURN a.name, type(r)
   ```

   ![image-20210111234110584](/Users/mac/Library/Application Support/typora-user-images/image-20210111234110584.png)

5. 查询所有有朋友关系的节点

   ```
   MATCH (n)-[:FRIENDS]-() RETURN n
   ```

   ![image-20210111234213002](/Users/mac/Library/Application Support/typora-user-images/image-20210111234213002.png)

6. 查找某人的朋友的朋友

   ```
   MATCH (a:Person {name:'Mike'})-[r1:FRIENDS]-()-[r2:FRIENDS]-(friend_of_a_friend) RETURN friend_of_a_friend.name AS fofName
   ```

   ![image-20210111234357410](/Users/mac/Library/Application Support/typora-user-images/image-20210111234357410.png)



## 2.3 图数据的增加/删除/修改

1. 增加/修改节点的属性

   ```
   MATCH (a:Person {name:'Liz'}) SET a.age=34;
   MATCH (a:Person {name:'Shawn'}) SET a.age=32;
   MATCH (a:Person {name:'John'}) SET a.age=44;
   MATCH (a:Person {name:'Mike'}) SET a.age=25;
   ```

2. 删除节点的属性

   ```
   MATCH (a:Person {name:'Mike'}) SET a.test='test';
   MATCH (a:Person {name:'Mike'}) REMOVE a.test
   ```

   删除属性操作主要通过`REMOVE`

3. 删除结点

   ```
   MATCH (a:Location {city:'Portland'}) DELETE a
   ```

   删除节点操作是`DELETE`

   > 注意：只能删除没有关系的节点。如果该节点有关系，需要先删除与其相连的关系，才能删除结点。

4. 删除有关系的节点

   ```
   MATCH (a:Person {name:'Todd'})-[rel]-(b:Person) DELETE a,b,rel
   ```

   如下图：

   ![image-20210111235730373](/Users/mac/Library/Application Support/typora-user-images/image-20210111235730373.png)



# 3. 通过Python操作Neo4j

首先需要安装neo4j和py2neo:

```
pip install neo4j
pip install py2neo
```

## 3.1 neo4j模块：执行CQL ( cypher ) 语句

```
# step 1：导入 Neo4j 驱动包
from neo4j import GraphDatabase
# step 2：连接 Neo4j 图数据库
driver = GraphDatabase.driver("bolt://localhost:7687", auth=("neo4j", "password"))
# 添加 关系 函数
def add_friend(tx, name, friend_name):
    tx.run("MERGE (a:Person {name: $name}) "
          "MERGE (a)-[:KNOWS]->(friend:Person {name: $friend_name})",
          name=name, friend_name=friend_name)
# 定义 关系函数
def print_friends(tx, name):
    for record in tx.run("MATCH (a:Person)-[:KNOWS]->(friend) WHERE a.name = $name "
                        "RETURN friend.name ORDER BY friend.name", name=name):
        print(record["friend.name"])
# step 3：运行
with driver.session() as session:
    session.write_transaction(add_friend, "Arthur", "Guinevere")
    session.write_transaction(add_friend, "Arthur", "Lancelot")
    session.write_transaction(add_friend, "Arthur", "Merlin")
    session.read_transaction(print_friends, "Arthur")
```

输出结果如下图，注意红框中填写的是数据库的url：

![image-20210112001410437](/Users/mac/Library/Application Support/typora-user-images/image-20210112001410437.png)



## 3.2 py2neo模块：通过操作python变量，达到操作neo4j的目的

```
# step 1：导包
from py2neo import Graph, Node, Relationship
# step 2：构建图
g = Graph("http://localhost:7474",auth=("neo4j","password"))
# step 3：创建节点
tx = g.begin()
a = Node("Person", name="Alice")
tx.create(a)
b = Node("Person", name="Bob")
# step 4：创建边
ab = Relationship(a, "KNOWS", b)
# step 5：运行
tx.create(ab)
tx.commit()
```

输出结果：

![image-20210112002034452](/Users/mac/Library/Application Support/typora-user-images/image-20210112002034452.png)



# 4. 通过csv文件批量导入图数据

前面学习的是单个创建节点，不适合大批量导入。这里练习的是使用neo4j-admin import命令导入，适合部署在docker环境下的neo4j。

1. 关闭neo4j

   ```
   neo4j stop
   ```

2. csv文件放在路径`import`下：

   如下图：

   ![image-20210112002815387](/Users/mac/Library/Application Support/typora-user-images/image-20210112002815387.png)

3. 导入到图数据库:

   使用neo4j-admin import工具只能往空数据库中导入数据，且csv文件必须在import目录下。使用csv文件导入数据时，每个结点都必须有一个唯一的ID类属性，但是最好不要起名为ID，这会和数据库本身维护的ID字段冲突。

```
bin/neo4j-admin import --nodes=/Users/mac/neo4j-community-4.2.2/import/nodes.csv --relationships=/Users/mac/neo4j-community-4.2.2/import/relations.csv  --delimiter=^ --database=neo4j-default.db
```

4. 指定neo4j使用哪个数据库

   ```
   修改 /root/neo4j/conf/neo4j.conf 文件中的 dbms.default_database=graph.db
   ```

5. 重启neo4j就可以看到数据已经导入成功了

