# 选择功能

## Introduce ##

## 监督学习

+ 属性重要性 ***ATTRIBUTE_IMPORTANCE***
	- 作用：确定在预测目标属性中最重要的属性
	- 举例：根据客户对信用卡计划的反馈，找到最重要的预测指标
	- 算法（ **ALGO_NAME**）：
		- [最小描述长度算法（DML）*ALGO\_AI\_MDL*](#最小描述长度)  默认算法
		- [奇异矩阵分解（CUR） *ALGO\_CUR\_DECOMPOSITION*](#CUR分解)

+ 分类 ***CLASSIFICATION***
	- 作用：将项目分配给离散类，并预测项目所属的类
	- 举例：给定有关一组客户的人口统计数据，预测客户对信用卡计划的反应
	- 算法（ **ALGO_NAME**）：
		- [朴素贝叶斯 *ALGO\_NAIVE\_BAYES *] (#朴素贝叶斯) 默认算法
		- [决策树  *ALGO\_DECISION\_TREE*](#决策树)
		- [显式语义分析 *ALGO\_EXPLICIT\_SEMANTIC\_ANALYS*](#显式语义分析)
		- [广义线性模型  *ALGO\_GENERALIZED\_LINEAR_MODEL*](#广义线性模型 )
		- [神经网络 *ALGO\_NEURAL\_NETWORK*](#神经网络)
		- [随机森林 *ALGO\_RANDOM\_FOREST*](#随机森林)
		- [支持向量机 *ALGO\_SUPPORT\_VECTOR\_MACHINES*](#支持向量机) 

+ 回归 ***REGRESSION***
	- 作用：预测连续值
	- 举例：根据有关一组客户的人口统计和购买数据，预测客户的年龄
	- 算法（ **ALGO_NAME**）：
		- [支持向量机 *ALGO\_SUPPORT\_VECTOR\_MACHINES*](#支持向量机)  默认算法
		- [广义线性模型  *ALGO\_GENERALIZED\_LINEAR_MODEL*](#广义线性模型)
	
## 无监督学习

+ 异常检测	
	- 作用：标识不符合“正常”数据特征的项目（异常值）
	- 举例：给定有关一组客户的人口统计数据，请确定与规范有显着差异的客户购买行为
	- 算法（ **ALGO_NAME**）：
		- [支持向量机 *ALGO\_SUPPORT\_VECTOR\_MACHINES*](#支持向量机)

+ 关联规则 ***ASSOCIATION***
	- 作用：查找倾向于在数据中同时出现的项目，并指定控制它们同时出现的规则
	- 举例：找到倾向于一起购买的物品并指定它们之间的关系
	- 算法（ **ALGO_NAME**）：
		- [Apriori算法 *ALGO\_APRIORI\_ASSOCIATION\_RULES*](#Apriori)

+ 聚类 ***CLUSTERING***
	- 作用：在数据中查找自然分组
	- 举例：将人口统计数据细分为聚类，并对个人属于给定聚类的可能性进行排名
	- 算法（ **ALGO_NAME**）：
		- [k-均值 *ALGO_KMEANS*](#k-均值) 默认算法
		- [O集群（正交分区聚类）  *ALGO\_O\_CLUSTER*](#O集群)


+ 特征提取 ***FEATURE_EXTRACTION***
	- 作用：创造新的 属性（特征）使用原始属性的线性组合
	- 举例：给定有关一组客户的人口统计数据，将属性分组为客户的一般特征
	- 算法（ **ALGO_NAME**）：
		- [非负矩阵分解 *ALGO\_NONNEGATIVE\_MATRIX\_FACTOR*](#非负矩阵分解) 默认算法 
		- [显式语义分析 *ALGO\_EXPLICIT\_SEMANTIC\_ANALYS*](#显式语义分析)
		- [奇异值分解 *ALGO\_SINGULAR\_VALUE\_DECOMP*](#奇异值分解)

+ 时间序列 ***TIME_SERIES***
	- 作用：时间序列是一种预测挖掘函数。时间序列模型预测在用户指定的时间窗口上按时间顺序排列的历史数值数据的未来值。时间序列模型采用指数平滑算法。
	- 举例：根据历史销售记录，预测未来一段时间的销售量
	- 算法（ **ALGO_NAME**）：
		- [指数平滑 *ALGO\_EXPONENTIAL\_SMOOTHING*](#指数平滑)