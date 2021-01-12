# 1. 介绍

​		本文尝试运行了一个基于医疗知识图谱的问答系统，项目源地址见[github](https://github.com/zhihao-chen/QASystemOnMedicalGraph)。本文内容分为搭建知识图谱、启动问答测试和问题总结三部分。



# 2. 搭建知识图谱

1. 启动Neo4j服务

   ```
   cd ./..../QASystemOnMedicalGraph-master
   bin/neo4j start
   ```

2. 运行以下命令：

   ```
   python build_graph.py
   ```

   该命令运行时间较长，请耐心等待。。。

3. 运行之后，打开浏览器输入：http://localhost:7474/browser/，可以看到我们导入的数据的知识图谱，如下：

   ![image-database](https://github.com/LemonRay/Datawhale-KnowledgeGraph-learning/raw/main/Pics/tak2-1.png)



# 3. 启动问答测试

1. 运行以下命令：

   ```
   python kbqa_test.py
   ```

2. 运行结果：

   ![image-test-question](https://github.com/LemonRay/Datawhale-KnowledgeGraph-learning/raw/main/Pics/task2-2.jpg)

   

# 4. 项目结构

- data：存放数据
- img：存放readme里的图片
- model：存放训练好的tfidf模型和意图识别模型
- build_graph.py：构建图
- entity_extractor.py：抽取问句中的实体和识别意图
- search_answer.py：根据不同的实体和意图构造cypher查询语句,查询图数据库并返回答案，详见task05



# 5. 问题总结

1. 在运行`build_graph.py`之前，需要修改该文件的红框部分（用户名及密码）：

   ![image-test-question](https://github.com/LemonRay/Datawhale-KnowledgeGraph-learning/raw/main/Pics/tak2-3.jpg)

2. 在运行`kbqa_test.py`之前，需要在`search_answer.py`里面修改数据库的用户名密码：

   ![image-task-4](https://github.com/LemonRay/Datawhale-KnowledgeGraph-learning/raw/main/Pics/tak2-4.png)

3. 问题1：
   ModuleNotFoundError: No module named 'ahocorasick'

   解决：

   ```
   pip install pyahocorasick -i https://pypi.tuna.tsinghua.edu.cn/simple/
   ```

4. 问题2：

   ModuleNotFoundError: No module named 'sklearn'

   解决：

   ```
   pip install scikit-learn==0.20.3
   ```

   > 注意，这里要指定安装版本，最新版本也不行

5. 问题3：
   ModuleNotFoundError: No module named 'jieba'

   解决：

   ```
   pip install jieba
   ```

6. 问题4：

   NameError: name 'data_dir' is not defined

   解决：在代码中加入下一行：

   ```
   data_dir = './data/'
   ```

   

