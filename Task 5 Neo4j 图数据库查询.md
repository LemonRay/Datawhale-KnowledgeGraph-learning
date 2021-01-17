# 1. 介绍

​		在[Task4](https://github.com/LemonRay/Datawhale-KnowledgeGraph-learning/blob/main/Task3-Neo4j图数据库导入数据.md)中，我们完成了**将用户输入问答系统的自然语言转化成知识库的查询语句**。Task5中将会完成**使用查询语句在知识图谱中查询到结果**的功能。本文参考[Datawhale 知识图谱组队学习 之 Task 5](https://github.com/datawhalechina/team-learning-nlp/blob/master/KnowledgeGraph_Basic/task05.md)



# 2. 主体类 AnswerSearching 框架介绍

```
class AnswerSearching:
    def __init__(self):
        pass
    # 主要是根据不同的实体和意图构造cypher查询语句
    def question_parser(self, data):
        """
        主要是根据不同的实体和意图构造cypher查询语句
        :param data: {"Disease":[], "Alias":[], "Symptom":[], "Complication":[]}
        :return:
        """
        pass
    # 将问题转变为cypher查询语句
    def transfor_to_sql(self, label, entities, intent):
        """
        将问题转变为cypher查询语句
        :param label:实体标签
        :param entities:实体列表
        :param intent:查询意图
        :return:cypher查询语句
        """
        pass
    # 执行cypher查询，返回结果
    def searching(self, sqls):
        """
        执行cypher查询，返回结果
        :param sqls:
        :return:str
        """
        pass
    # 根据不同意图，返回不同模板的答案
    def answer_template(self, intent, answers):
        """
        根据不同意图，返回不同模板的答案
        :param intent: 查询意图
        :param answers: 知识图谱查询结果
        :return: str
        """
        pass
```



# 3. 分模块介绍

1. 根据不同的实体和意图构造cypher查询语句

   ```
   def question_parser(data):
           """
           主要是根据不同的实体和意图构造cypher查询语句
           :param data: {"Disease":[], "Alias":[], "Symptom":[], "Complication":[]}
           :return:
           """
           sqls = []
           if data:
               for intent in data["intentions"]:
                   sql_ = {}
                   sql_["intention"] = intent
                   sql = []
                   if data.get("Disease"):
                      sql = transfor_to_sql("Disease", data["Disease"], intent)
                   elif data.get("Alias"):
                       sql = transfor_to_sql("Alias", data["Alias"], intent)
                   elif data.get("Symptom"):
                       sql = transfor_to_sql("Symptom", data["Symptom"], intent)
                   elif data.get("Complication"):
                       sql = transfor_to_sql("Complication", data["Complication"], intent)
   
                   if sql:
                       sql_['sql'] = sql
                       sqls.append(sql_)
           return sql
   ```

2. 将问题转变为cypher查询语句

   ```
   def transfor_to_sql(label, entities, intent):
           """
           将问题转变为cypher查询语句
           :param label:实体标签
           :param entities:实体列表
           :param intent:查询意图
           :return:cypher查询语句
           """
           if not entities:
               return []
           sql = []
   
           # 查询症状
           if intent == "query_symptom" and label == "Disease":
               sql = ["MATCH (d:Disease)-[:HAS_SYMPTOM]->(s) WHERE d.name='{0}' RETURN d.name,s.name".format(e)
                      for e in entities]
           # 查询治疗方法
           if intent == "query_cureway" and label == "Disease":
               sql = ["MATCH (d:Disease)-[:HAS_DRUG]->(n) WHERE d.name='{0}' return d.name,d.treatment," \
                      "n.name".format(e) for e in entities]
            # 查询治疗周期
           if intent == "query_period" and label == "Disease":
               sql = ["MATCH (d:Disease) WHERE d.name='{0}' return d.name,d.period".format(e) for e in entities
           ...
   ```

3. 执行cypher查询，返回结果

   ```
   def searching(sqls):
           """
           执行cypher查询，返回结果
           :param sqls:
           :return:str
           """
           final_answers = []
           for sql_ in sqls:
               intent = sql_['intention']
               queries = sql_['sql']
               answers = []
               for query in queries:
                   ress = graph.run(query).data()
                   answers += ress
               final_answer = answer_template(intent, answers)
               if final_answer:
                   final_answers.append(final_answer)
           return final_answers
   ```

4. 根据不同意图，返回不同模板的答案

   ```
    def answer_template(intent, answers):
           """
           根据不同意图，返回不同模板的答案
           :param intent: 查询意图
           :param answers: 知识图谱查询结果
           :return: str
           """
           final_answer = ""
           if not answers:
               return ""
           # 查询症状
           if intent == "query_symptom":
               disease_dic = {}
               for data in answers:
                   d = data['d.name']
                   s = data['s.name']
                   if d not in disease_dic:
                       disease_dic[d] = [s]
                   else:
                       disease_dic[d].append(s)
               i = 0
               for k, v in disease_dic.items():
                   if i >= 10:
                       break
                   final_answer += "疾病 {0} 的症状有：{1}\n".format(k, ','.join(list(set(v))))
                   i += 1
               ...
   ```

   