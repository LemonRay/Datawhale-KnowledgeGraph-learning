# 1. 介绍

​	本文是对项目[QASystemOnMedicalGraph](https://github.com/zhihao-chen/QASystemOnMedicalGraph)的数据导入功能的解析。数据导入功能是通过调用`build_graph.py`文件实现的。`build_graph.py`文件主要包含MedicalGraph类。

# 2. MedicalGraph类介绍

1. 各函数功能：

```
class MedicalGraph:
    def __init__(self):
        # cur_dir：当前项目所处路径
        # self.data_path：数据文件存储的路径
        # self.graph：创建一个py2neo.database.Graph对象，链接 Neo4j 图数据库，注意指定数据库的用户名和密码
    
    # 读取文件，获得实体，实体关系
    def read_file(self):
        pass
    # 创建节点
    def create_node(self, label, nodes):
        pass
    # 创建疾病节点的属性
    def create_diseases_nodes(self, disease_info):
        pass
    # 创建知识图谱实体
    def create_graphNodes(self):
        pass
    # 创建实体关系边
    def create_graphRels(self):
        pass
    # 创建实体关系边
    def create_relationship(self, start_node, end_node, edges, rel_type, rel_name):
        pass
```

2. 函数之间的调用关系

   （1）创建知识图谱实体（节点）：
   		def create_graphNodes()
   		-> self.read_file()		# 从csv文件中读取实体、关系等数据
   		-> self.create_diseases_nodes(disease_info)		# 创建带有属性的疾病节点

   ​		-> self.create_node(self, label, nodes)		# 创建节点，并为每个节点指定标签

   （2）创建实体之间的关系（边）：

   ​		def create_graphRels()：
   ​		-> self.read_file()		# 从csv文件中读取实体、关系等数据

   ​		-> self.create_relationship(start_node, end_node, edges, rel_type, rel_name)		# 创建实体关系边	