# 算法设置

## Introduce ##

## 通用设置

[Oracle文档  Global Settings](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-24047A09-0542-4870-91D8-329F28B0ED75)


| 	设置字段	 | 	<div style="width: 7rem">设置名称<div>	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ODMS\_MISSING\_VALUE\_TREATMENT	 | 	缺失值处理	 | 	ODMS\_MISSING\_VALUE\_MEAN\_MODE<br>ODMS\_MISSING\_VALUE\_DELETE\_ROW<br>*ODMS\_MISSING\_VALUE\_AUTO*	 | 指示如何处理训练数据中的缺失值。 <br>此设置不会影响应用数据。 <br><small>**ODM\_MISSING\_VALUE\_MEAN\_MODE** 在创建模型和应用模型时，用均值（数字）或众数（属性）替换缺失值。 <br>**ODMS\_MISSING\_VALUE\_AUTO**针对不同的算法执行不同的策略。<br> **ODMS\_MISSING\_VALUE\_DELETE\_ROW** 将删除训练数据中包含缺失值的行。 但是，如果要在应用数据中使用同样的策略，则必须明确指定转换规则。ODMS\_MISSING\_VALUE\_DELETE\_ROW 适用于所有算法。</small> | 
| 	ODMS\_TEXT\_POLICY\_NAME	 | 	标记提取	 | 	使用CTX\_DDL.CREATE\_POLICY 创建的Oracle Text POLICY的名称	 | 	此设置影响如何从非结构化文本中提取单个标记。<br>有关CTX\_DDL.CREATE\_POLICY 的详细信息 ，请参见《 Oracle Text Reference》 。	 | 
| 	ODMS\_TEXT\_MAX\_FEATURES	 | 	最大属性数	 | 	正整数<br>*默认值为 3000<br>ESA算法的默认值为 300000*	 | 	从文档集传递到CREATE\_MODEL的在所有文本属性中的最大相异属性数 。 	 | 
| 	ODMS\_TEXT\_MIN\_DOCUMENTS	 | 	文本单元最少出现次数	 | 	非负整数<br>*默认值为 1<br>ESA算法的默认值为 3*	 | 	这是一个文本处理设置，用于限制一个单元需要在多少文档中出现，才能作为特征。	 | 
| 	ODMS\_PARTITION\_COLUMNS	 | 	分区列	 | 	逗号分隔列表	 | 	此设置用于构建分区模型。 设置值是逗号分隔列表，包含挖掘属性，用于确定分区键值。 这些挖掘属性是从输入列中获取的，除非将 XFORM\_LIST 参数传递给 CREATE\_MODEL 。 如果将 XFORM\_LIST 参数传递给 CREATE\_MODEL ，则挖掘属性将从这些转换产生的属性中获取。	 | 
| 	ODMS\_MAX\_PARTITIONS	 | 	最大分区数	 | 	整数 [1,1000000]<br>*默认值为 1000*	 | 	限制模型允许的最大分区数	 | 
| 	ODMS\_SAMPLING	 | 	启用采样	 | 	ODM\_SAMPLING\_ENABLE<br>*ODMS\_SAMPLING\_DISABLE*	 | 	该设置允许用户对建模数据进行采样。	 | 
| 	ODMS\_SAMPLE\_SIZE	 | 	采样行数	 | 	正整数<br>*默认值是由系统确定的*	 | 	此设置确定要采样的行数（大约）。 仅在 ODMS\_SAMPLING 启用后才能设置 。	 | 
| 	ODMS\_PARTITION\_BUILD\_TYPE	 | 	分区构建方式	 | 	ODMS\_PARTITION\_BUILD\_INTRA<br>ODMS\_PARTITION\_BUILD\_INTER<br>*ODMS\_PARTITION\_BUILD\_HYBRID*	 | 	此设置控制分区模型的并行构建方式。 <br><small>**ODMS\_PARTITION\_BUILD\_INTRA** — 每个分区都使用所有从属服务并行构建。 <br>**ODMS\_PARTITION\_BUILD\_INTER** — 每个分区完全构建在单个从属服务中，但是由于多个从属服务处于活动状态，因此可以同时构建多个分区。<br>**ODMS\_PARTITION\_BUILD\_HYBRID** — 它是其他两种类型的组合，大多数情况下建议采用，以适应动态环境。</small>	 | 
| 	ODMS\_TABLESPACE\_NAME	 | 	存储空间	 | 	*如果用户不提供此设置，则用户的默认表空间将创建结果模型内容*	 | 	此设置控制存储说明。 如果用户将其显式设置为表空间的名称（具有足够的配额），则指定的表空间存储将创建结果模型内容。 "	 | 
| 	ODMS\_RANDOM\_SEED	 | 	随机种子	 | 	非负整数<br>*默认值为 0*	 | 	具有随机数种子的哈希函数会生成具有均匀分布的随机数。 用户可以通过此设置控制随机数种子。<br>随机森林、神经网络和CUR使用此设置。	 | 
| 	ODMS\_DETAILS	 | 	启用生成详情视图	 | 	*ODMS\_ENABLE*<br>ODMS\_DISABLE	 | 	此设置减少了创建模型（尤其是分区模型）时使用的空间。<br><small>设置为ODMS\_ENABLE时，它将在创建模型时创建模型表和视图。 <br> 设置 为ODMS\_DISABLE 时，不会创建模型视图，也不会创建与模型详细信息相关的表。空间的减少取决于模型。 减少约10倍。</small>	 | 


## Apriori

### 概念解释

[Oracle文档  Apriori](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/apriori.html#GUID-B7D12599-FB4C-45E3-BCE4-E54A3C6F0E64)

+ 关联挖掘问题可以分解为以下子问题：
	- 查找以指定的最小频率发生的一组交易中的所有项目组合。这些组合称为频繁项集。
	- 计算频繁项目集中项目可能同时出现的规则。
+ 建议不要使用关联规则挖掘来查找涉及问题域中包含大量项目的罕见事件的关联。
Apriori发现频率高于最小支持阈值的模式。因此，要查找涉及罕见事件的关联，该算法必须以非常低的最小支持值运行。但是，这样做可能会使枚举的项目集数量激增，尤其是在项目数量很大的情况下。这大大增加了执行时间。当数据具有大量属性时，分类或异常检测更适合发现罕见事件。

### 参数设置

[Oracle文档  Global Settings](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-24047A09-0542-4870-91D8-329F28B0ED75)

| 	设置字段	 | 	<div style="width: 7rem">设置名称</div>	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_APRIORI\_ASSOCIATION\_RULES*	 | 	算法名称	 | 
| 	ODMS\_ITEM\_ID\_COLUMN\_NAME	 | 	商品类别列名称	 | 	列名称	 | 	交易记录中表示商品类别的列名称。 指定此设置后，算法期望数据包括两列： <br><small>Case ID，分类或数字 ；<br>Item ID，分类或数字 ；</small><br>交易数据的典型示例是购物篮数据，其中Case ID代表可能包含许多商品的购物篮。 每个Item存储在单独的行中，可能需要很多行来表示一个Case。 Case ID值不能唯一标识每一行。   | 
| 	ODMS\_ITEM\_VALUE\_COLUMN\_NAME	 | 	商品数量列名称	 | 	列名称	 | 	包含与交易中每个项目相关联的值的列的名称。 仅当指定了用于 ODMS\_ITEM\_ID\_COLUMN\_NAME 指示数据以本机事务格式表示 的值时，才使用此设置 。 如果 使用了ASSO\_AGGREGATES ，则数据必须包括以下三列以及在AGGREGATES设置中指定的列。<br><small>Case ID，分类或数字；<br>Item ID，分类或数字， 由ODMS\_ITEM\_ID\_COLUMN\_NAME指定；<br>Item Value， 分类或数字，由ODMS\_ITEM\_VALUE\_COLUMN\_NAME指定；</small><br>如果ASSO\_AGGREGATES/Case ID和Item ID列存在，则“Item Value”列可能会出现，也可能不会出现。 “Item Value”列可以指定诸如数量（例如，3个苹果）或类型（例如，苹果电脑）之类的信息。	 | 

## CUR分解

### 概念解释

[Oracle文档  CUR Matrix Decomposition](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/cur-matrix-decomposition.html#GUID-EABE920F-1196-49C0-89CD-F25E062E16EF)

+ CUR矩阵分解是一种低秩矩阵分解算法，明确表达在数据矩阵的少量实际列和/或实际行中。

+ CUR矩阵分解的开发是为了替代奇异值分解（SVD）和主成分分析（PCA）。 CUR矩阵分解从数据矩阵中选择具有较高统计杠杆作用或较大影响的列和行。
通过实施CUR矩阵分解算法，可以从原始数据矩阵中识别出少数几个最重要的属性和/或行。 因此，CUR矩阵分解是探索性数据分析的重要工具。 
CUR矩阵分解可应用于各种领域，并有助于回归，分类和聚类。

### 参数设置

[Oracle文档  Algorithm Settings: CUR Matrix Decomposition](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-D42F5952-1C59-431D-BACE-21C8C82AAE41)

| 	设置字段	 | 	设置名称	 | 	设定值(标红是默认值)	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	<span style="width:8rem">算法名称</span>	 | 	*ALGO\_CUR\_DECOMPOSITION*	 | 		 | 
| 	CURS\_APPROX\_ATTR_NUM	 | 	属性数量	 | 	正整数。<br> *默认是属性数*	 | 	定义要选择的属性的大概数量。 	 | 
| 	CURS\_ROW\_IMPORTANCE	 | 	是否选择行	 | 	CURS\_ROW\_IMP\_ENABLE<br>*CURS\_ROW\_IMP\_DISABLE*	 | 	定义指示是否执行行选择。 	 | 
| 	CURS\_APPROX\_ROW_NUM	 | 	选择行数	 | 	正整数。<br> *默认值为总行数*	 | 	定义要选择的大约行数。 仅当用户设置行选择（ CURS_ROW_IMP_ENABLE ） 时才使用此参数	 | 
| 	CURS\_SVD\_RANK	 | 	等级参数	 | 	正整数。<br> *默认该值由系统确定*	 | 	定义列/行杠杆得分计算中使用的等级参数。 	 | 


## 决策树

### 概念解释

[Oracle文档  Decision Tree](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/decision-tree.html#GUID-14DE1A88-220F-44F0-9AC8-77CA844D4A63)

+ 像朴素贝叶斯一样，决策树算法也基于条件概率。与朴素贝叶斯不同，决策树生成规则。规则是人类可以理解的条件语句，可以在数据库中用于标识一组记录。
+ 在某些应用中，解释决定原因的能力可能至关重要。例如，市场营销专业人员需要对客户群的完整描述才能启动成功的市场营销活动。决策树算法是此类应用程序的理想选择。
+ 使用决策树规则来验证模型。如果规则对业务专家有意义，那么这将证实模型的有效性。

### 参数设置

[Oracle文档   Algorithm Settings: Decision Tree](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-03435110-D723-42FD-B4EA-39C86A039566)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	------------	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	<span style="width:20rem">算法名称</span>	 | 	*ALGO\_DECISION\_TREE*	 | 	算法名称	 | 
| 	TREE\_IMPURITY\_METRIC	 | 	不纯度的度量方式	 | 	TREE\_IMPURITY\_ENTROPY<br>*TREE\_IMPURITY\_GINI*	 | 	决策树不纯度的度量方式。树算法寻求拆分每个节点的最佳问题。 最佳拆分器和拆分值是那些导致节点中实体的目标值同质性（纯度）增加最大的拆分器和拆分值。 纯度根据度量标准进行测量。 决策树可以使用基尼（ TREE\_IMPURITY\_GINI ）或熵（ TREE\_IMPURITY\_ENTROPY ）作为纯度度量。	 | 
| 	TREE\_TERM\_MAX\_DEPTH	 | 	最大树深度	 | 	 整数 [2,20]，<br> *默认值为 7* 	 | 	拆分条件：最大树深度（根与任何叶节点（包括叶节点）之间的最大节点数）	 | 
| 	TREE\_TERM\_MINPCT\_NODE	 | 	节点最少行数占比	 | 	 整数 [0,10]，<br> *默认值为 0.05 ，表示0.05％*	 | 	节点中数据行的最小数量，表示为训练数据中行的百分比	 | 
| 	TREE\_TERM\_MINPCT\_SPLIT	 | 	拆分节点所需的最少行数占比	 | 	 整数 (0,20]，<br> *默认值为 0.1 ，表示0.1％*	 | 	拆分节点所需的最小行数，以训练行的百分比表示 	 | 
| 	TREE\_TERM\_MINREC\_NODE	 | 	节点最少行数	 | 	正整数，<br> *默认值为 10* 	 | 	节点中的最小行数。 	 | 
| 	TREE\_TERM\_MINREC\_SPLIT	 | 	父节点中最少行数	 | 	正整数 > 1 <br> *默认值为 20*	 | 	拆分条件：父节点中最小记录数。 如果记录数低于此值，则不尝试拆分	 | 
| 	TREE\_TERM\_MAX\_DEPTH	 | 	属性的最多分组数	 | 	整数 [2,254]，<br> *默认值为 32*	 | 	"此参数指定每个属性的最多分组数。  请参阅 DBMS\_DATA\_MINING —自动数据准备"	 | 
| 	CLAS\_MAX\_SUP\_BINS	 | 	属性的最多分组数	 | 	整数 [2,2147483647]，<br> *默认值为 32* 	 | 	"此参数指定每个属性的最大箱数。 请参阅 DBMS\_DATA\_MINING —自动数据准备"	 | 


## 期望最大化

### 概念解释

[Oracle文档  Expectation Maximization](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/expectation-maximization.html#GUID-F4D117F3-FA0C-4CA4-9034-67D12339AE90)

+ 混合模型的期望最大化（EM）估计是一种流行的概率密度估计技术，Oracle Data Mining使用EM来实现**基于分布的聚类**算法（EM聚类）。

+ 期望最大化是一种迭代方法。它从初始参数猜测开始。参数值用于计算当前模型的可能性。这是**期望阶段**。然后重新计算参数值以使可能性最大化。这是**最大化阶段**。新的参数估计值用于计算新的期望值，然后再次对其进行优化以使可能性最大化。这个迭代过程一直持续到模型收敛为止。
+ 在密度估计中，目标是构建一个密度函数，以捕获给定总体的分布方式。在概率密度估计中，密度估计是基于表示总体样本的观察数据。模型中高数据密度的区域对应于基础分布的峰值。
+ 基于密度的聚类在概念上与基于距离的聚类（例如k -Means）不同。
在基于距离的聚类中，重点放在最小化群集间距离和最大化群集内距离。基于密度的聚类可以计算聚类分配中的可靠概率。它还可以自动处理缺失值。

### 参数设置

[Oracle文档   Algorithm Settings: Expectation Maximization](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-1796B451-BE1B-43BC-9839-05F5F73031C8)

|设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
|----	 | 	----	 | 	----	 | 	----	 | 
|ALGO\_NAME	 | 	<div style="width: 8rem">算法名称</div>	 | 	*ALGO\_EXPECTATION\_MAXIMIZATION*	 | 	算法名称	 | 
|EMCS\_ATTRIBUTE\_FILTER	 | 	是否启用属性筛选	 | 	EMCS\_ATTR\_FILTER\_ENABLE<br> EMCS\_ATTR\_FILTER\_DISABLE<br>*默认值是系统确定的* | 	是否在模型中包括不相关的属性。 当 EMCS\_ATTRIBUTE\_FILTER 启用时，不相关的属性不包括在内。注意： 此设置仅适用于未嵌套的属性。| 
|EMCS\_MAX\_NUM\_ATTR\_2D	 | 	最大属性数	 | 	文本整数 >=1<br>*默认值为 50* 	 | 	模型中要包含的最大相关属性数。 注意：此设置仅适用于未嵌套（2D）的属性。  | 
|EMCS\_NUM\_DISTRIBUTION	 | 	数字分布	 | 	EMCS\_NUM\_DISTR\_BERNOULLI<br> EMCS\_NUM\_DISTR\_GAUSSIAN<br> *EMCS\_NUM\_DISTR\_SYSTEM*	 | 	用于数字属性建模的分布。 应用于输入表或整个视图，并且不允许按属性指定。 选项包括伯努利分布、高斯分布或系统确定的分布。 选择伯努利分布或高斯分布时，所有数值属性均使用相同的分布类型建模。 当分布是系统确定的时，取决于数据，各个属性可以使用不同的分布（伯努利或高斯分布）。 | 
|EMCS\_NUM\_EQUIWIDTH\_BINS	 | 	等宽分组数	 | 	文本整数 (1,255]<br>*默认值为 11* 	 | 	用于收集数字列的群集统计信息的等宽分组数。 	 | 
|EMCS\_NUM\_PROJECTIONS	 | 	映射数	 | 	文本整数 >=1 <br>*默认值为 50*	 | 	指定将用于每个嵌套列的映射数。 如果一列的相异属性少于指定数量的映射，则将不会映射数据。 该设置适用于所有嵌套列。 | 
|EMCS\_NUM\_QUANTILE\_BINS	 | 	分位数分组数	 | 	文本整数 (1,255]<br> *默认值是系统确定的*	 | 	指定用于对多值伯努利分布的数字列进行建模的分位数分组的数量。	 | 
|EMCS\_NUM\_TOPN\_BINS	 | 	TOP N分组数	 | 	文本整数 (1,255]<br> *默认值是系统确定的*	 | 	指定用于对多值伯努利分布的数字列进行建模的前N个容器的数量	 | 
|EMCS\_CONVERGENCE\_CRITERION	 | 	收敛准则	 | 	EMCS\_CONV\_CRIT\_HELDASIDE<br>EMCS\_CONV\_CRIT\_BIC<br> *默认值是系统确定的	* | 	EM的收敛准则。 收敛准则可以基于保留的数据集，也可以是贝叶斯信息准则。 默认值是系统确定的。| 
|EMCS\_LOGLIKE\_IMPROVEMENT	 | 	似然值提升率	 | 	文本小数 (0,1)<br>*默认值为 0.001* 	 | 	当收敛标准基于保留数据集（ EMCS\_CONVERGENCE\_CRITERION = EMCS\_CONV\_CRIT\_HELDASIDE ）时，此设置指定向模型中添加新组分所需的对数似然函数的值的百分比提高	 | 
|EMCS\_NUM\_COMPONENTS	 | 	最大组分数	 | 	文本整数 >=1<br>*默认值为20* | 	模型中的最大组分数。如果启用了模型搜索，则算法会根据似然函数的改进或基于正则化（最多指定的最大值）自动确定组分数。 组分的数量必须大于或等于集群的数量	 | 
|EMCS\_NUM\_ITERATIONS	 | 	最大迭代次数	 | 	文本整数 >=1<br>*默认值为 100* 	 | 	指定EM算法中的最大迭代次数	 | 
|EMCS\_MODEL\_SEARCH	 | 	启用模型搜索	 | 	EMCS\_MODEL\_SEARCH\_ENABLE<br>*EMCS\_MODEL\_SEARCH\_DISABLE*	 | 	通过此设置，可以在EM中搜索模型，在EM中可以探索不同的模型尺寸并选择最佳尺寸	 | 
|EMCS\_REMOVE\_COMPONENTS	 | 	启用组分筛选	 | 	*EMCS\_REMOVE\_COMPS\_ENABLE*<br>EMCS\_REMOVE\_COMPS\_DISABLE | 	此设置允许EM算法从解决方案中删除一小部分	 | 
|EMCS\_RANDOM\_SEED	 | 	随机种子	 | 	非负整数<br> *默认值为 0* 	 | 	此设置控制EM中使用的随机生成器的种子	 | 
|EMCS\_CLUSTER\_COMPONENTS	 | 	启用组分分群	 | 	*EMCS\_\_CLUSTER\_COMP\_ENABLE*<br>EMCS\_CLUSTER\_COMP\_DISABLE	 | 	启用或禁用EM组分分组到高阶群集。禁用时，组分本身将视为群集。启用组分集群后，通过SQL CLUSTER 函数进行模型应用，将分配到更高级别的集群。 禁用群集时，CLUSTER 函数将按原始组分分组	 | 
|EMCS\_CLUSTER\_THRESH	 | 	分群阈值	 | 	文本整数 >=1<br> *默认值为 2* 	 | 	设置EM组分群集的差异阈值。 当相异性度量值小于阈值时，这些组分将合并为一个群集。 较低的阈值可能会产生更多更紧凑的群集。 较高的阈值可能会产生较少的群集，分布更广	 | 
|EMCS\_LINKAGE\_FUNCTION	 | 	相似度度量	 | 	*EMCS\_LINKAGE\_SINGLE*<br>EMCS\_LINKAGE\_AVERAGE<br>EMCS\_LINKAGE\_COMPLETE	 | 	允许为聚类步骤指定链接函数。EMCS\_LINKAGE\_SINGLE 使用分支内最近的距离。 簇倾向于更大并且具有任意形状。 EMCS\_LINKAGE\_AVERAGE 使用分支内的平均距离。 连锁效果较小，群集更紧凑。 EMCS\_LINKAGE\_COMPLETE 使用分支内的最大距离。 群集较小，并且需要强大的组分重叠	 | 
|EMCS\_CLUSTER\_STATISTICS	 | 	收集统计信息	 | 	*EMCS\_CLUS\_STATS\_ENABLE*<br>EMCS\_CLUS\_STATS\_DISABLE	 | 	启用或禁用收集聚类的描述性统计信息（质心，直方图和规则）。 禁用统计信息后，模型大小将减小，并且 GET\_MODEL\_DETAILS\_EM 仅返回分类规则（层次结构）和集群计数	 | 
|EMCS\_MIN\_PCT\_ATTR\_SUPPORT	 | 	最小支持率	 | 	文本小数 (0 ,1)<br>*默认值为 0.1* 	 | 	属性包含在集群规则中所需的最低支持率。支持率是分配给群集的数据行的百分比，该数据行必须具有属性的非空值	 | 


## 显式语义分析

#### 概念解释

[Oracle文档  Explicit Semantic Analysis](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/explicit-semantic-analysis.html#GUID-7DC30272-E234-4C7C-B7D2-29D0E5448BA6)

+ 显式语义分析（ESA）是用于**特征提取**的一种无监督算法，从Oracle Database 18c开始，ESA被增强为**分类**的监督算法。
+ 作为一种特征提取算法，ESA不会发现潜在特征，而是使用现有知识库中表示的显式特征。ESA作为一种特征提取算法，主要用于**计算文本文档的语义相似度**和显式主题建模。
+ 作为一种分类算法，ESA主要用于**对文本文档进行分类**。ESA的特征提取和分类版本也可以应用于数字和分类输入数据。

### 参数设置

[Oracle文档   Algorithm Settings: Explicit Semantic Analysis](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-91FC543B-24E4-4D93-9D79-E95B1846F3B7)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_EXPLICIT\_SEMANTIC\_ANALYS*	 | 	算法名称	 | 
| 	ESAS\_VALUE\_THRESHOLD	 | 	权重阈值	 | 	非负数<br>*默认值为 1e-8*	 | 	此设置为属性权重设置一个较小的阈值	 | 
| 	ESAS\_MIN\_ITEMS	 | 	最少频次	 | 	*默认文本输入 100*<br>*默认非文本输入为 0*	 | 	此设置确定输入行中需要出现的非零条目的最小数量	 | 
| 	ESAS\_TOPN\_FEATURES	 | 	最大特征数	 | 	正整数<br> *默认值为 1000*	 | 	此设置限制每个属性的最大特征数量	 | 

## 指数平滑

### 概念解释

[Oracle文档  Exponential Smoothing](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/expnential-smoothing.html#GUID-65C7E533-E403-4F71-A5FE-EC034745904F)

+ 指数平滑方法广泛用于**预测**。

+ 指数平滑方法已在半个多世纪的预测中广泛使用。它在战略，战术和运营级别都有应用程序。
例如，在战略层面上，预测用于预测投资回报，增长和创新效果。在战术层面，预测用于预测成本，库存需求和客户满意度。在操作级别上，预测用于设置目标并预测质量和对标准的符合性。
+ 在最简单的形式中，指数平滑是一种具有单个参数的移动平均值方法，该模型模拟了过去水平对未来值的指数下降影响。
通过各种扩展，指数平滑涵盖了比竞争对手更广泛的模型类别，例如Box-Jenkins自回归综合移动平均（ARIMA）方法。Oracle Data Mining使用最新的状态空间方法来实现指数平滑，该方法结合了单一错误源（SSOE）假设，从而提供了理论和性能上的优势。

+ 指数平滑扩展到以下内容：
	- 混合并匹配错误类型（加法或乘法），趋势（加法，乘法或无）和季节性（加法，乘法或无）的模型矩阵
	- 趋势衰减的模型。
	- 直接处理不规则时间序列和缺少值的时间序列的模型。
	
### 参数设置

[Oracle文档   Algorithm Settings: Exponential Smoothing Models](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-A95A0A38-8A5A-4470-B49F-80D81C588BFC)
	
| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_EXPONENTIAL\_SMOOTHING*	 | 	算法名称	 | 
| 	EXSM\_MODEL	 | 	指数平滑模型	 | 	*EXSM\_SIMPLE*<br>EXSM\_SIMPLE\_MULT<br>EXSM\_HOLT<br>EXSM\_HOLT\_DMP<br>EXSM\_MUL\_TRND<br>EXSM\_MULTRD\_DMP<br>EXSM\_SEAS\_ADD<br>EXSM\_SEAS\_MUL<br>EXSM\_HW<br>EXSM\_HW\_DMP<br>EXSM\_HW\_AD<br>EXSM\_HW\_DS	 | 	此设置指定模型。<br>EXSM\_SIMPLE 简单的指数平滑模型。<br>EXSM\_SIMPLE\_MULT 有乘性误差的简单指数平滑模型。<br>EXSM\_HOLT 霍尔特线性指数平滑模型。<br>EXSM\_HOLT\_DMP 有阻尼趋势的霍尔特线性指数平滑模型。<br>EXSM\_MUL\_TRND 有乘性趋势的指数平滑模型。<br>EXSM\_MULTRD\_DMP 有乘性阻尼趋势的指数平滑模型。<br>EXSM\_SEAS\_ADD 有加性的季节，但无趋势的指数平滑模型。<br>EXSM\_SEAS\_MUL 有乘性的季节，但无趋势的指数平滑模型<br>EXSM\_HW 霍尔特-温特斯三重指数平滑模型，加性的趋势，乘性的季节。<br>EXSM\_HW\_DMP 霍尔特-温特斯三重指数平滑模型，加性的趋势，乘性的季节，有阻尼。<br>EXSM\_HW\_ADDSEA 霍尔特-温特斯三重指数平滑模型，加性的趋势，加性的季节。<br>EXSM\_DHW\_ADDSEA 霍尔特-温特斯三重指数平滑模型，加性的趋势，加性的季节，有阻尼。<br>EXSM\_HWMT 霍尔特-温特斯三重指数平滑模型，乘性的趋势，加性的季节。<br>EXSM\_HWMT\_DMP 霍尔特-温特斯三重指数平滑模型，乘性的趋势，加性的季节，有阻尼。| 
| 	EXSM\_SEASONALITY	 | 	季节周期	 | 	正整数 >1	 | 	此设置指定为季节性周期的长度。 所取值必须大于 1 。 <br>例如，设置值 4 意味着每四个观测值形成一个季节性周期。<br> 此设置仅适用于有季节性的模型，否则会报错。 未设置EXSM\_INTERVAL，该设置应用于原始输入的时间序列。 有 EXSM\_INTERVAL 设置时，该设置适用于累积的时间序列。	 | 
| 	EXSM\_INTERVAL	 | 	时间单位	 | 	EXSM\_INTERVAL\_YEAR<br>EXSM\_INTERVAL\_QTR<br>EXSM\_INTERVAL\_MONTH<br>EXSM\_INTERVAL\_WEEK<br>EXSM\_INTERVAL\_DAY<br>EXSM\_INTERVAL\_HOUR<br>EXSM\_INTERVAL\_MIN<br>EXSM\_INTERVAL\_SEC	 | 	仅当时间列（ case\_id 列）是日期时间类型时，才能应用设置 。 它指定时间序列的间隔单位。 <br>如果输入的时间列为日期时间类型，且EXSM\_INTERVAL 未设置，将报错。 如果输入表的时间列为oracle数字类型，且 EXSM\_INTERVAL 有设置，将报错。"	 | 
| 	EXSM\_ACCUMULATE	 | 	聚合方式	 | 	EXSM\_ACCU\_TOTAL<br>EXSM\_ACCU\_STD<br>EXSM\_ACCU\_MAX<br>EXSM\_ACCU\_MIN<br>EXSM\_ACCU\_AVG<br>EXSM\_ACCU\_MEDIAN<br>EXSM\_ACCU\_COUNT	 | 	仅当时间列具为日期时间类型时，才提供此设置。 它指定如何从输入时间序列中生成累积时间序列的值。	 | 
| 	EXSM\_SETMISSING	 | 	缺失值处理	 | 	EXSM\_MISS\_MIN<br>EXSM\_MISS\_MAX<br>EXSM\_MISS\_AVG<br>EXSM\_MISS\_MEDIAN<br>EXSM\_MISS\_LAST<br>EXSM\_MISS\_FIRST<br>EXSM\_MISS\_PREV<br>EXSM\_MISS\_NEXT<br>*EXSM\_MISS\_AUTO*	 | 	此设置指定如何处理缺失值，这些值可能来自输入数据 和/或 聚合的时间序列。 它可以指定数字或选项。 如果指定了数字，则所有缺少的值都将设置为该数字。 <br>EXSM\_MISS\_MIN ：用聚合时间序列的最小值替换缺失值。<br>EXSM\_MISS\_MAX ：用聚合时间序列的最大值替换缺失值。<br>EXSM\_MISS\_AVG ：用聚合时间序列的平均值替换缺失值。<br>EXSM\_MISS\_MEDIAN ：用聚合时间序列的中位数替换缺失值。<br>EXSM\_MISS\_LAST ：用聚合时间序列的最后一个非缺失值替换缺失值。<br>EXSM\_MISS\_FIRST ：用聚合时间序列的第一个非缺失值替换缺失值。<br>EXSM\_MISS\_PREV ：用聚合时间序列的前一个非缺失值替换缺失值。<br>EXSM\_MISS\_NEXT ：用聚合时间序列的后一个非缺失值替换缺失值。<br>EXSM\_MISS\_AUTO ：将输入数据视为不规则（非均匀间隔）时间序列,将缺失值视为缺口。	 | 
| 	EXSM\_PREDICTION\_STEP	 | 	预测长度	 | 	整数 [1,30]<br>*默认值为 1*	 | 	此设置用于指定要预测的长度。大于30 会报错。	 | 
| 	EXSM\_CONFIDENCE\_LEVEL	 | 	置信度	 | 	小数(0,1)<br>*默认置信度为 95%*	 | 	此设置用于指定预测置信度。 结果包含指定置信区间的下限和上限	 | 
| 	EXSM\_OPT\_CRITERION	 | 	优化准则	 | 	EXSM\_OPT\_CRIT\_LIKE<br> EXSM\_OPT\_CRIT\_MSE<br> EXSM\_OPT\_CRIT\_AMSE<br>EXSM\_OPT\_CRIT\_SIG<br> EXSM\_OPT\_CRIT\_MAE 	 | 	此设置用于指定优化准则。<br>EXSM\_OPT\_CRIT\_LIKE  似然值<br>EXSM\_OPT\_CRIT\_MSE 均方误差（Mean Square Error）<br>EXSM\_OPT\_CRIT\_AMSE 均方平均误差（Average mean square error）<br>EXSM\_OPT\_CRIT\_SIG 显著性（significance）E<br>XSM\_OPT\_CRIT\_MAE 平均绝对误差（Mean Absolute Error）	 | 
| 	EXSM\_NMSE	 | 	均方误差窗口	 | 	正整数	 | 	此设置指定用于计算误差度量均方平均误差（AMSE）的窗口的长度。	 | 


## 广义线性模型

### 概念解释

[Oracle文档  Generalized Linear Models](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/generalized-linear-models.html#GUID-5E59530F-EBD9-414E-8C8B-63F8079772CE)

+ GLM包括并扩展了线性模型的类别。线性模型做出一组限制性假设，最重要的是，目标（因变量y）正态分布是基于具有恒定方差的预测变量的值，而与预测响应值无关。线性模型的优点及其局限性包括计算的简便性，可解释的模型形式以及计算有关拟合质量的某些诊断信息的能力。

+ 广义线性模型放宽了这些限制，这些限制在实践中经常被违反。例如，二进制（是/否或0/1）响应在类之间没有相同的方差。此外，线性模型中的项之和通常可以具有非常大的范围，包括非常负的值和非常正的值。对于二进制响应示例，我们希望响应为[0,1]范围内的概率。
+ 广义线性模型通过两种机制来适应违反线性模型假设的响应：链接函数和方差函数。
	- 链接函数将目标范围转换为潜在的-无限到+无限，从而可以保持线性模型的简单形式。
	- 方差函数将方差表示为预测响应的函数，从而适应具有非恒定方差的响应（例如二进制响应）。
+ Oracle数据挖掘包括GLM模型家族中两个最受欢迎的成员，它们具有最流行的链接和差异函数：
	- 同一性链接和方差函数等于常数1（响应值范围内的恒定方差）的**线性回归**。
	- 具有Logit链接和二项式方差函数的**Logistic回归**。
	
### 参数设置

[Oracle文档   Algorithm Settings: Generalized Linear Models](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-4E3665B9-B1C2-4F6B-AB69-A7F353C70F5C)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	<div style="width: 10rem">算法名称</div>	 | 	*ALGO\_GENERALIZED\_LINEAR_MODEL*	 | 	算法名称	 | 
| 	GLMS\_CONF\_LEVEL	 | 	置信度	 | 	文本小数(0,1)<br>*默认的置信度为 0.95* 	 | 	回归系数置信区间的置信度	 | 
| 	GLMS\_FTR\_SELECTION	 | 	启用特征选择	 | 	GLMS\_FTR\_SELECTION\_ENABLE<br>*GLMS\_FTR\_SELECTION\_DISABLE*	 | 	是否启用特征选择	 | 
| 	GLMS\_FTR\_SEL\_CRIT	 | 	特征选择准则	 | 	GLMS\_FTR\_SEL\_AIC<br>GLMS\_FTR\_SEL\_SBIC<br>GLMS\_FTR\_SEL\_RIC<br>GLMS\_FTR\_SEL\_ALPHA\_INV<br> 启用特征选择后，*算法会根据数据自动选择惩罚标准*	 | 	特征选择的惩罚标准。	 | 
| 	GLMS\_MAX\_FEATURES	 | 	最大特征数	 | 	文本整数 (0,2000]<br>默认情况下，算法会*限制特征数量*以节省内存	 | 	启用特征选择后，此设置指定最终模型的最大特征数	 | 
| 	GLMS\_FTR\_GENERATION	 | 	启用特征生成	 | 	GLMS\_FTR\_GENERATION\_ENABLE<br>*GLMS\_FTR\_GENERATION\_DISABLE*	 | 	是否启用特征生成。 默认不启用。 注意： 仅当同时启用了特征选择时，才能启用特征生成。	 | 
| 	GLMS\_FTR\_GEN\_METHOD	 | 	特征生成方式	 | 	GLMS\_FTR\_GEN\_QUADRATIC<br>GLMS\_FTR\_GEN\_CUBIC<br>启用特征生成后，算法会根据数据自动选择最合适的特征生成方法	 | 	特征生成是平方还是立方	 | 
| 	GLMS\_PRUNE\_MODEL	 | 	启用特征修剪	 | 	GLMS\_PRUNE\_MODEL\_ENABLE<br>GLMS\_PRUNE\_MODEL\_DISABLE<br>启用特征选择后，*算法会根据数据自动执行修剪*	 | 	是否对最终模型启用特征修剪。 修剪规则，线性回归基于T检验，逻辑回归基于Wald检验。 算法循环剔除特征，直到所有特征对整个数据集在统计上都显著	 | 
| 	GLMS\_REFERENCE\_CLASS\_NAME	 | 	类别列	 | 	目标值<br>默认情况下，算法会为参考*类别选择频次最高*（大多数情况下）的值 | 	在二元逻辑回归模型中用作类别的目标值。 产生另一类的概率。 	 | 
| 	GLMS\_RIDGE\_REGRESSION	 | 	启用岭回归	 | 	GLMS\_RIDGE\_REG\_ENABLE<br>GLMS\_RIDGE\_REG\_DISABLE	 | 	启用或禁用岭回归。 <br>Ridge适用于回归和分类函数。启用ridge时， PREDICTION\_BOUNDS SQL函数不会产生预测界限 。 <br>注意 ：仅当未启用特征选择或明确禁用特征选择时，才可以启用Ridge。 如果同时启用了岭回归和特征选择，则会引发异常。 | 
| 	GLMS\_RIDGE\_VALUE	 | 	岭回归参数	 | 	文本数字 >0	 | 	ridge参数的值。 仅当启用“岭回归”时才使用此设置。 如果算法在内部启用了Ridge回归，则*ridge参数由算法确定*	 | 
| 	GLMS\_ROW\_DIAGNOSTICS	 | 	启用行诊断	 | 	GLMS\_ROW\_DIAG\_ENABLE<br>GLMS\_ROW\_DIAG\_DISABLE	 | 	启用或禁用行诊断。	 | 
| 	GLMS\_CONV\_TOLERANCE	 | 	收敛判据	 | 	(0, 1 )<br>*默认值是系统确定的*	 | 	设置收敛判据	 | 
| 	GLMS\_NUM\_ITERATIONS	 | 	最大迭代次数	 | 	正整数<br>*默认值是系统确定的*	 | 	最大迭代次数	 | 
| 	GLMS\_BATCH\_ROWS	 | 	解算器批处理数量	 | 	0 或正整数<br>*默认是 2000*	 | 	SGD解算器的批处理行数。 此参数设置SGD解算器的批处理大小。 输入0将根据数据估计批量	 | 
| 	GLMS\_SOLVER	 | 	选择解算器	 | 	GLMS\_SOLVER\_SGD<br>GLMS\_SOLVER\_CHOL<br>GLMS\_SOLVER\_QR<br>GLMS\_SOLVER\_LBFGS\_ADMM<br>*默认值是系统确定的*	 | 	此设置允许用户选择解算器。 如果启用了特征选择，则无法选择求解器 。 <br>GLMS\_SOLVER\_SGD (随机梯度下降)<br>GLMS\_SOLVER\_CHOL (Cholesky分解)<br>GLMS\_SOLVER\_QR<br>GLMS\_SOLVER\_LBFGS\_ADMM	 | 
| 	GLMS\_SPARSE\_SOLVER	 | 	启用稀疏求解器	 | 	GLMS\_SPARSE\_SOLVER\_ENABLE<br>*GLMS\_SPARSE\_SOLVER\_DISABLE*	 | 	启用或禁用稀疏求解器（如果可用）	 | 
| 	ODMS\_ROW\_WEIGHT\_COLUMN\_NAME	 | 	行加权因子	 | 	列名称	 | 	训练数据中包含行加权因子的列名称。 列数据类型必须为NUMBER。<br>行权重可以用作重复行的紧凑表示，例如在实验设计中，特定配置重复多次。 行权重还可用于在模型构建过程中强调某些行。 例如，将模型偏向于使用较新的行，并远离过时的数据。 | 

## k-均值

### 概念解释

[Oracle文档  k-Means](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/k-means.html#GUID-AA5D4D4E-936F-474A-8919-5E7FF5EE69B1)

+ ķ -Means算法是基于距离的将数据划分为指定数量的聚类算法。基于距离的算法依靠距离函数来度量案例之间的相似性。根据使用的距离函数将案例分配给最近的聚类。

+ Oracle Data Mining实现了k -Means算法的增强版本，具有以下功能：
	- **距离函数**：该算法支持欧几里得距离和余弦距离函数。默认值为欧几里得。
	- **可扩展并行模型构建**：该算法使用基于Bahmani，Bahman等人的非常有效的初始化方法。
	- **群集属性**：对于每个群集，算法都会返回质心，每个属性的直方图，以及描述超级框的规则，该规则将分配给该群集的大多数数据括起来。重心报告分类属性的模式，并报告数字属性的均值和方差。
+ 这种用于k -Means的方法避免了建立多个k -Means模型的需要，并提供了始终优于传统k -Means的聚类结果。

### 参数设置

[Oracle文档   Algorithm Settings: k-Means](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-7010593E-C323-4DFC-8468-D85CE41A0C3C)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	<div style="width: 6rem">算法名称</div>	 | 	*ALGO\_KMEANS*	 | 	算法名称	 | 
| 	KMNS\_CONV\_TOLERANCE	 | 	收敛判据	 | 	小数 (0,1)<br>*默认值为 0.001*	 | 	设定收敛判据的大小。 <br>算法迭代，直到满足收敛判据或最大迭代次数为止 。 <br>减小收敛判据会产生更准确的解决方案，但可能导致运行时间更长	 | 
| 	KMNS\_DISTANCE	 | 	距离度量	 | 	KMNS\_COSINE<br>*KMNS\_EUCLIDEAN*	 | 	设定距离函数 。<br>KMNS\_COSINE ：余弦距离<br>KMNS\_EUCLIDEAN ：欧氏距离	 | 
| 	KMNS\_ITERATIONS	 | 	最大迭代次数	 | 	正整数<br>*默认的迭代次数为 20*	 | 	最大迭代次数 。 <br>算法迭代，直到满足收敛判据或最大迭代次数为止 	 | 
| 	KMNS\_MIN\_PCT\_ATTR\_SUPPORT	 | 	最小支持度	 | 	小数 [0,1]<br>*默认的最低支持为 0.1*	 | 	设置属性的最小支持度，即非空属性值的最小百分比，大于该阈值的属性会包含在聚类规则中。 <br>如果数据稀疏或包含许多缺失值，此时最小支持值太高会导致聚类规则太少	 | 
| 	KMNS\_NUM\_BINS	 | 	属性分组数	 | 	正整数<br>直方图分组的*默认数量为 1*	 | 	设置属性直方图中生成的分组数。 <br>每个属性的分组边界是在整个数据集上全局计算求得。<br>分组方式是等宽的。<br>所有属性具有相同数量的分组，只有单个值的属性只有一个分组	 | 
| 	KMNS\_SPLIT\_CRITERION	 | 	分隔准则	 | 	KMNS\_SIZE<br>*KMNS\_VARIANCE*	 | 	聚类的分割标准 。<br> 分割标准控制新聚类的初始位置。 算法构建一棵二叉树，每次次添加一个新聚类。当拆分标准是基于样本量大小时，新聚类将放在当前最大聚类所在的区域中。 当拆分标准是基于方差时，新聚类将放置在最分散的聚类区域中	 | 
| 	KMNS\_RANDOM\_SEED	 | 	随机种子	 | 	非负整数<br>*默认值为 0*	 | 	此设置控制初始化时使用的随机生成器的种子 。 	 | 
| 	KMNS\_DETAILS	 | 	生成聚类详情	 | 	KMNS\_DETAILS\_NONE <br>*KMNS\_DETAILS\_HIERARCHY*<br>KMNS\_DETAILS\_ALL	 | 	此设置确定在构建过程中计算的聚类信息的详细级别。 <br>KMNS\_DETAILS\_NONE ：不计算聚类的详细信息。 只保留评分信息。 <br>KMNS\_DETAILS\_HIERARCHY ：计算聚类层次结构和记录数。 这是默认值。 <br>KMNS\_DETAILS\_ALL ：计算聚类层次结构、记录数、描述性统计信息（均值，方差，最频属性，直方图和规则）。	 | 


## 最小描述长度

### 概念解释

[Oracle文档  Minimum Description Length](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/minimum-description-length.html#GUID-96A38D67-2F09-4659-976D-4DDF478555E0)

+ 最小描述长度（MDL）是信息理论模型的选择原则。它是信息论（信息量化研究）和学习理论（基于经验数据的泛化能力研究）中的重要概念。

+ MDL假定最简单、最紧凑的数据表示形式是对数据的最佳、最可能的解释。MDL原理用于构建Oracle数据挖掘属性重要性模型。

<div id="朴素贝叶斯"></div> 

## 朴素贝叶斯

### 概念解释

[Oracle文档  Naive Bayes](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/naive-bayes.html#GUID-BB77D68D-3E07-4522-ACB6-FD6723BDA92A)

+ 朴素贝叶斯可用于*二分类*和*多分类*问题。朴素贝叶斯算法基于条件概率。它使用贝叶斯定理， 通过计算历史数据中值的频率和值的组合来计算概率的公式。

+ 给定已经发生的另一事件的概率，贝叶斯定理找到事件发生的概率。如果B代表从属事件并A代表先前事件，则贝叶斯定理可表示如下。
	- **概率（B给定A）=概率（A和B）/概率（A）**
+ 为了计算B给定的概率A，该算法对A和B一起发生的情况进行计数，然后将其除以A单独发生的情况的数目。

### 参数设置

[Oracle文档   Algorithm Settings: Naive Bayes](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-A04C5F4E-1303-44DC-A7DA-185C969330C8)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_NAIVE\_BAYES*	 | 	算法名称	 | 
| 	NABS\_PAIRWISE\_THRESHOLD	 | 	成对阈值	 | 	小数 [0,1]<br>*默认值为0*	 | 	算法的成对阈值。两个事件同时发生的概率阈值	 | 
| 	NABS\_SINGLETON\_THRESHOLD	 | 	单例阈值	 | 	小数 [0,1]<br>*默认值为0* | 	算法的单例阈值。事件单独发生的概率阈值	 | 

## 神经网络

### 概念解释

[Oracle文档  Neural Network](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/neural-network.html#GUID-C45971D9-A874-4546-A0EC-1FF25B229E2B)

+ 神经网络在Oracle数据挖掘中用于*分类*和*回归*。

+ 在机器学习中，人工神经网络是一种受生物神经网络启发的算法，用于估计或近似依赖于大量通常未知输入的函数。人工神经网络由大量相互连接的神经元组成，它们相互之间交换消息以解决特定问题。
他们通过示例进行学习，并在学习过程中调整神经元之间连接的权重。神经网络能够解决各种各样的任务，例如计算机视觉，语音识别和各种复杂的业务问题。

### 参数设置

[Oracle文档   Algorithm Settings: Neural Network](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-7793F608-2719-45EA-87F9-6F246BA800D4)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_NEURAL\_NETWORK*	 | 	算法名称	 | 
| 	NNET\_HIDDEN\_LAYERS	 | 	隐藏层数	 | 	非负整数<br> *默认值为1*	 | 	通过隐藏层数定义拓扑	 | 
| 	NNET\_NODES\_PER\_LAYER	 | 	每层节点数	 | 	逗号分隔正整数列表<br>每层的*默认节点数是属性数*，最大为50	 | 	"通过每层节点数定义拓扑。 不同的层可以具有不同数量的节点。<br> 该值应为非负整数，并用逗号分隔。 例如，“ 10,20,5”。<br>  设置值必须与隐藏层数一致"	 | 
| 	NNET\_ACTIVATIONS	 | 	每层激活函数	 | 	*NNET\_ACTIVATIONS\_LOG\_SIG*<br> NNET\_ACTIVATIONS\_LINEAR<br>NNET\_ACTIVATIONS\_TANH<br>NNET\_ACTIVATIONS\_ARCTAN<br>NNET\_ACTIVATIONS\_BIPOLAR\_SIG	 | 	定义隐藏层的激活函数。 例如，'''NNET\_ACTIVATIONS\_BIPOLAR\_SIG''， '''NNET\_ACTIVATIONS\_TANH''。<br>不同的层可以有不同的激活函数。 激活函数的数量必须与NNET\_NODES\_PER\_LAYER和 NNET\_NODES\_PER\_LAYER  一致。<br>注意：所有引号都是单引号，两个单引号用于转义SQL语句中的单引号。 | 
| 	NNET\_WEIGHT\_LOWER\_BOUND	 | 	权重下限	 | 	实数<br>*默认值为 –sqrt(6/(l\_nodes+r\_nodes))* <br><small><br> l\_nodes 的值 ：<br>输入层密集属性为（ 1+密集属性数量 ） <br>输入层稀疏属性为 稀疏属性数量 <br>每个隐藏层为（ 1+该隐藏层中的节点数）<br>r\_nodes的值是权重连接的层中的节点数。</small>	 | 	该设置指定权重随机初始化的区域下限。 <br> NNET\_WEIGHT\_LOWER\_BOUND 和 NNET\_WEIGHT\_UPPER\_BOUND 必须一起设置。 只设置一个将引发错误。 <br> NNET\_WEIGHT\_LOWER\_BOUND 不得大于 NNET\_WEIGHT\_UPPER\_BOUND 。  | 
| 	NNET\_WEIGHT\_UPPER\_BOUND	 | 	权重上限	 | 	实数<br>*默认值为 sqrt(6/(l\_nodes+r\_nodes))* 	 | 	该设置指定权重随机初始化的区域上限。<br>NNET\_WEIGHT\_LOWER\_BOUND 和 NNET\_WEIGHT\_UPPER\_BOUND 必须一起设置。 只设置一个将引发错误。<br>NNET\_WEIGHT\_LOWER\_BOUND 不得大于 NNET\_WEIGHT\_UPPER\_BOUND 。	 | 
| 	NNET\_ITERATIONS	 | 	最大迭代次数	 | 	正整数<br>*默认值为 200*	 | 	指定神经网络算法的最大迭代次数	 | 
| 	NNET\_TOLERANCE	 | 	收敛判据	 | 	小数 (0,1）<br>*默认值为 0.000001* 	 | 	定义收敛判据	 | 
| 	NNET\_REGULARIZER	 | 	正则化规则	 | 	 - NNET\_REGULARIZER\_L2<br>- 如果训练行的总数不大于50000，则默认值为*NNET\_REGULARIZER\_NONE*<br> - 如果训练行的总数大于50000，则默认值为*NNET\_REGULARIZER\_HELDASIDE*  | 	神经网络算法的正则化设置。<br>NNET\_REGULARIZER\_NONE：无正则化<br>NNET\_REGULARIZER\_L2 ：L2范数正则化<br>NNET\_REGULARIZER\_HELDASIDE：验证集测试	 | 
| 	NNET\_HELDASIDE\_RATIO	 | 	HELDASIDE保留率	 | 	小数 [0,1]<br>*默认值为 0.25*	 | 	定义held-aside方法的保留率	 | 
| 	NNET\_HELDASIDE\_MAX\_FAIL	 | 	HELDASIDE停止窗口	 | 	正整数<br>*默认值为6*	 | 	正则化使用 NNET\_REGULARIZER\_HELDASIDE 时，如果网络性能在验证数据上无法提高，或在连续的 NNET\_HELDASIDE\_MAX\_FAIL 时期内保持不变，则训练过程会提前停止	 | 
| 	NNET\_REG\_LAMBDA	 | 	L2正则化参数λ	 | 	非负数<br>*默认值为1*	 | 	定义L2正则化参数λ。 不能与 NNET\_REGULARIZER\_HELDASIDE一起设置	 | 


## 非负矩阵分解

### 概念解释

[Oracle文档  Non-Negative Matrix Factorization](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmcon/non-negative-matrix-factorization.html#GUID-76F89641-E1D3-4B11-8319-4A152389D510)
+ 非负矩阵分解（Non-Negative Matrix Factorization，NMF）是一种先进的特征提取算法。
当存在许多属性并且属性模棱两可或可预测性较弱时，NMF很有用。通过组合属性，NMF可以产生有意义的模式、话题或主题。
+ NMF创建的每个功能都是原始属性集的线性组合。每个特征都有一组系数，这些系数是特征上每个属性权重的度量。有一个单独的每个数字属性和每个分类属性的每个不同值的系数。系数都是非负的。
+ NMF特别适合 文本挖掘。在文本文档中，同一个单词可以出现在不同位置的不同含义。
例如，“加息”可以应用于户外或利率。通过组合属性，NMF引入了上下文，这对于解释功能至关重要：
	- “远足” +“山地”->“户外运动”
	- “加息” +“利息”->“利率”

### 参数设置

[Oracle文档   Algorithm Settings: Neural Network](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-E5CBCC4F-819A-4BD8-9C2B-BC4A9EE870AD)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_NONNEGATIVE\_MATRIX\_FACTOR*	 | 	算法名称	 | 
| 	NMFS\_CONV\_TOLERANCE	 | 	收敛判据	 | 	小数 (0,0.5]<br>*默认是 0.05*	 | 	NMF算法的收敛判据	 | 
| 	NMFS\_NONNEGATIVE\_SCORING	 | 	允许负特征值	 | 	*NMFS\_NONNEG\_SCORING\_ENABLE*<br>NMFS\_NONNEG\_SCORING\_DISABLE	 | 	在评分结果中是否允许使用负数。 <br>NMFS\_NONNEG\_SCORING\_ENABLE：负特征值将替换为零。<br>NMFS\_NONNEG\_SCORING\_DISABLE：允许使用负特征值。	 | 
| 	NMFS\_NUM\_ITERATIONS	 | 	迭代次数	 | 	整数 [1,500]<br>*默认是 50*	 | 	算法的迭代次数	 | 
| 	NMFS\_RANDOM\_SEED	 | 	随机种子	 | 	整数<br>*默认值为 –1*	 | 	算法的随机种子	 | 

## O集群

### 概念解释

[Oracle文档   O-Cluster](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-12C659A2-0317-47C8-A05D-708FAF7DF370)
+ O-Cluster是一种快速，可扩展的基于网格的聚类算法，非常适合挖掘大型，高维数据集。该算法可以产生高质量的簇，而无需依赖用户定义的参数。
+ O-Cluster的目的是识别数据中的高密度区域，并将密集区域分成多个簇。它使用轴平行的一维（正交）数据投影来识别密度区域。该算法寻找分裂点，这些分裂点会导致不重叠且大小平衡的不同簇。
+ O-Cluster通过创建二叉树层次结构来递归操作。叶簇的数量是自动确定的。可以将算法配置为限制最大群集数。

### 参数设置

[Oracle文档  Algorithm Settings: O-Cluster](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-12C659A2-0317-47C8-A05D-708FAF7DF370)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_O\_CLUSTER*	 | 	算法名称	 | 
| 	OCLT\_SENSITIVITY	 | 	拆分峰值密度	 | 	小数 [0,1]<br>*默认值为 0.5*	 | 	小数，指定分离新簇所需的峰值密度。 该分数与整体均匀密度有关	 | 


## 随机森林

### 概念解释

[Oracle文档   Random Forest](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/random-forest.html#GUID-B6506C33-8555-4181-993F-CD7D48B4DA3C)

+ 随机森林是Oracle数据挖掘使用的**分类**算法。该算法构建树的**合集**（也称为**forest**）。
+ 该算法建立了许多决策树模型，并使用集合进行预测。通过从训练数据集中选择一个随机样本作为输入，可以构建一个单独的决策树。
在树的每个节点上，仅选择预测变量的随机样本来计算分割点。这引入了森林中不同树木使用的数据的差异。
参数RFOR\_SAMPLING\_RATIO和RFOR\_MTRY用于指定样本大小和在每个节点上选择的预测变量数量。用户可以ODMS\_RANDOM\_SEED在运行算法之前用于设置随机种子值。

### 参数设置

[Oracle文档  Algorithm Settings: Algorithm Settings: Random Forest](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-481B6C67-B26E-4689-AD4C-98062D5A2117)

| 	设置字段	 | 	设置名称	 | 	<div style="width: 14rem">设定值</div>	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_RANDOM\_FOREST*	 | 	算法名称	 | 
| 	TREE\_TERM\_MAX\_DEPTH	 | 	最大树深度	 | 	 整数 [2,100]<br>*默认值为16*	 | 	拆分条件：最大树深度（根与任何叶节点（包括叶节点）之间的最大节点数）	 | 
| 	RFOR\_MTRY	 | 	拆分子集最小样本量	 | 	正整数<br>*默认值为模型特征列中的一半*	 | 	在节点上选择拆分点时，考虑的随机列子集的大小。 对于每个节点，池的大小不变，但是特定的候选列会有变化。  特殊值 0 表示候选池包括所有列。	 | 
| 	RFOR\_NUM\_TREES	 | 	树数量	 | 	整数 [1,65535]<br>默认值为 20	 | 	森林中的树数量	 | 
| 	RFOR\_SAMPLING\_RATIO	 | 	种树抽样比例	 | 	小数 (0,1]<br>*默认值为训练集样本的一半*	 | 	用于构建单个树的随机抽样的数据比例	 | 

## 奇异值分解

### 概念解释

[Oracle文档   Singular Value Decomposition](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/singular-value-decomposition.html#GUID-703B237F-D9C5-4543-97DD-31A914BB6A05)
+ 奇异值分解（SVD）及其密切相关的主成分分析（PCA）是一种行之有效的特征提取方法，具有广泛的应用范围。
+ Oracle Data Mining将SVD实现为特征提取算法，将PCA实现为SVD模型的特殊评分方法。
+ SVD和PCA是正交线性变换，最适合捕获数据的基础方差。此属性对于减少高维数据的维数和支持有意义的数据可视化非常有用。
+ SVD和PCA除了减少尺寸外，还有许多重要的应用。这些包括矩阵求逆，数据压缩和未知数据值的插补。

### 参数设置

[Oracle文档  Algorithm Constants and Settings: Singular Value Decomposition](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-684B3705-A314-458B-A6D9-3191DF376117)

| 	设置字段	 | 	<div style="width: 8rem">设置名称</div>	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_SINGULAR\_VALUE\_DECOMP*	 | 	算法名称	 | 
| 	SVDS\_MAX\_NUM\_FEATURES	 | 	最大特征数	 | 	2500	 | 	SVD支持的最大特征数。	 | 
| 	SVDS\_U\_MATRIX\_OUTPUT	 | 	保留U矩阵	 | 	SVDS\_U\_MATRIX\_ENABLE<br>*SVDS\_U\_MATRIX\_DISABLE*	 | 	是否保留 SVD生成 的 U 矩阵。<br>SVD中的U矩阵具有与构建数据中的行数一样多的行。 为节省空间，默认不保留该矩阵 。<br>当 SVDS\_U\_MATRIX\_OUTPUT 启用时，生成的数据必须包含Case ID。 如果不存在Case ID，并且要求U矩阵，则会引发异常。	 | 
| 	SVDS\_SCORING\_MODE	 | 	评分模式	 | 	*SVDS\_SCORING\_SVD*<br>SVDS\_SCORING\_PCA	 | 	对模型使用SVD评分还是PCA评分。<br>使用SVD对构建数据进行评分时，投影将与U矩阵相同。 使用PCA对构建数据进行评分时，投影是U和S矩阵的乘积	 | 
| 	SVDS\_SOLVER	 | 	选择解算器	 | 	SVDS\_SOLVER\_TSSVD SVDS\_SOLVER\_TSEIGEN SVDS\_SOLVER\_SSVD SVDS\_SOLVER\_STEIGEN<br>*默认求解器类型由数据决定*	 | 	此设置指定用于计算数据SVD的解算器。<br>对于PCA，解算器设置指定用于计算数据的PCA的SVD求解器的类型。<br>如果属性的数量大于3240，则使用默认的宽解算器。 否则，将选择默认的窄解算器。<br><small> 对于窄数据的求解器：<br> 高瘦SVD使用QR计算TSVD（ SVDS\_SOLVER\_TSSVD ），最多8100个属性；<br>高瘦SVD使用特征值计算，TSEIGEN（ SVDS\_SOLVER\_TSEIGEN ），适用于最多包含11500个属性，是窄数据的默认求解器。 <br> 对于宽解算器（适用于多达一百万个属性的矩阵）：<br>随机SVD使用QR计算SSVD（ SVDS\_SOLVER\_SSVD ），是宽数据求解器的默认求解器。 <br> 随机SVD使用特征值计算STEIGEN（ SVDS\_SOLVER\_STEIGEN ）。</small>  | 
| 	SVDS\_TOLERANCE	 | 	修剪特征阈值	 | 	范围[ 0, 1 ]<br>*默认值为数据决定*	 | 	此设置用于修剪特征。 将要素的特征值的最小值定义为不修剪的第一个特征值的份额	 | 
| 	SVDS\_RANDOM\_SEED	 | 	随机种子	 | 	范围[ 0 - 4,294,967,296 ]<br>*默认值为 0*	 | 	随机种子用于确定初始化随机SVD求解器使用的采样矩阵。SVD解算器必须设置为 SSVD 或 STEIGEN 。	 | 
| 	SVDS\_OVER\_SAMPLING	 | 	采样列数	 | 	范围[ 1, 5000 ]	 | 	此设置决定随机SVD解算器使用的采样矩阵的列数。 此矩阵中的列数等于请求的要素数加上过采样设置。 SVD解算器必须设置为 SSVD 或 STEIGEN 。	 | 
| 	SVDS\_POWER\_ITERATIONS	 | 	幂迭代	 | 	范围[ 0, 20 ]<br>*默认值为 2*	 | 	幂迭代设置提高了SSVD解算器的精度。SVD解算器必须设置为 SSVD 或 STEIGEN 。	 | 


## 支持向量机

### 概念解释

[Oracle文档   Support Vector Machines](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/support-vector-machines.html#GUID-FD5DF1FB-AAAA-4D4E-84A2-8F645F87C344)

+ 支持向量机（SVM）是一种强大的，先进的算法，具有基于Vapnik-Chervonenkis理论的强大理论基础。SVM具有强大的正则化属性。正则化是指模型对新数据的概括。
+ 因为SVM通常被视为专家的工具，该算法通常需要数据准备，调整和优化。
Oracle Data Mining最小化了这些要求。您不需要成为专家就可以在Oracle Data Mining中构建高质量的SVM模型。例如：
	- 在大多数情况下，不需要数据准备。
	- 默认调整参数通常就足够了。

### 参数设置

[Oracle文档  Algorithm Settings: Support Vector Machine](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmapi/DBMS_DATA_MINING.html#GUID-12408982-E738-4D0F-A2BC-84D895E07ABB)

| 	设置字段	 | 	设置名称	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ALGO\_NAME	 | 	算法名称	 | 	*ALGO\_SUPPORT\_VECTOR\_MACHINES*	 | 	算法名称	 | 
| 	SVMS\_COMPLEXITY\_FACTOR	 | 	复杂度因子	 | 	文本数字 >0<br>*默认值是算法根据数据估算*	 | 	复杂度因子，正则化设置可以在模型的复杂性和模型的适应性之间取得平衡，从而对新数据实现良好的概括。 <br>SVM使用数据驱动的方法来查找复杂性因素。用于SVM算法的分类和回归	 | 
| 	SVMS\_CONV\_TOLERANCE	 | 	收敛判据	 | 	文本数字 >0<br>*默认值为 0.0001*	 | 	SVM算法的收敛判据	 | 
| 	SVMS\_EPSILON	 | 	正则化设置	 | 	文本数字 >0<br>*默认值为 0.1*	 | 	回归的正则化设置，类似于复杂度因子。 Epsilon指定数据中的允许残差或噪声。 用于SVM回归	 | 
| 	SVMS\_KERNEL\_FUNCTION	 | 	核函数	 | 	SVMS\_GAUSSIAN<br>*SVMS\_LINEAR*	 | 	支持向量机的核函数。 线性或高斯。 	 | 
| 	SVMS\_OUTLIER\_RATE	 | 	异常率	 | 	文本数字 (0,1)<br>*默认值为 0.01*	 | 	训练数据中期望的异常值率。 仅对单类SVM模型有效（异常检测）	 | 
| 	SVMS\_STD\_DEV	 | 	高斯核函数标准偏差	 | 	文本数字 >0<br>*默认值由算法根据数据估算*	 | 	控制高斯核函数的扩散。 <br>SVM使用数据驱动的方法来查找与典型案例之间的距离具有相同比例的标准偏差值。 SVM算法的标准偏差值。仅适用于高斯内核	 | 
| 	SVMS\_NUM\_ITERATIONS	 | 	最大迭代次数	 | 	正整数<br>*默认值是系统确定*	 | 	SVM迭代次数上限。默认值是系统确定的，因为它取决于SVM解算器。	 | 
| 	SVMS\_NUM\_PIVOTS	 | 	Cholesky分解最大主元数	 | 	范围[ 1; 10000 ]<br>*默认值为 200*	 | 	不完全Cholesky分解中使用的主元数量的上限。 只能为非线性内核设置。	 | 
| 	SVMS\_BATCH\_ROWS	 | 	解算器批处理数量	 | 	正整数<br>*默认值为 20000*	 | 	此设置适用于具有线性内核的SVM模型。 此设置设置SGD解算器的批处理大小。 输入0将触发数据驱动的批量估计。	 | 
| 	SVMS\_REGULARIZER	 | 	正则化规则	 | 	SVMS\_REGULARIZER\_L1<br>SVMS\_REGULARIZER\_L2<br>*默认值是系统确定*	 | 	此设置控制SGD解算器使用的正则化类型。 该设置仅可用于线性SVM模型。默认值是系统确定的，因为它取决于潜在的模型大小。	 | 
| 	SVMS\_SOLVER	 | 	选择解算器	 | 	SVMS\_SOLVER\_SGD （次梯度下降）<br>SVMS\_SOLVER\_IPM （内点法）<br>*默认值是系统确定*	 | 	此设置允许用户选择SVM解算器。如果内核是非线性的，则不能选择SGD求解器。	 | 


