# 模型设置

## Introduce ##

## 通用模型设置（规范方法）

[Oracle文档 45.7.11 CREATE_MODEL Procedure](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-7F525A4E-9C93-44D6-BFFF-10BC018CD4A6)

+ 模型名称	***model\_name***
	- 模型的名称，格式为\[ schema\_name \.\] model_name 。 如果未指定schema，则使用您自己的schema。 有关模型命名限制，请参见使用说明。
+ 功能	***mining\_function***
	- 模型功能，见[功能 mining\_function](#功能mining_function)
+ 数据表名称 ***data\_table\_name***
	- 包含模型训练数据的表或视图
+ Case\_Id列名称	***case\_id\_column\_name***
	- 构建数据中的案例标识符列。
+ 目标列名称	***target\_column\_name***
	- 对于监督模型，训练集中的目标列。 NULL 适用于无监督模型。
+ 设置表名称	***settings\_table\_name***
	- 该表包含模型的设置。 如果全部使用默认设置，可以用NULL不指定设置表。
+ *可选-*数据schema名称	***data\_schema\_name***
	- 训练集数据所在的schema。 如果为 NULL ，则使用您自己的schema。
+ *可选-*设置schema名称	***settings\_schema\_name***
	- 设置表数据所在的schema。 如果为 NULL ，则使用您自己的schema。
+ xform_list	***xform\_list***
	- 根据 PREP_AUTO 设置的值，在自动转换之外或代替自动转换而使用的转换列表 。

示例：

	<copy>
	DECLARE V_XLST DBMS_DATA_MINING_TRANSFORM.TRANSFORM_LIST;
	
	DBMS_DATA_MINING.CREATE_MODEL (
		  model_name            => 'my_model',
		  mining_function       => DBMS_DATA_MINING.REGRESSION,
		  data_table_name       => 'train_data',
		  case_id_column_name   => 'case_id',
		  target_column_name    => 'target',
		  settings_table_name   => 'my_model_setting',
		  data_schema_name      => 'my_schema',
		  settings_schema_name  => 'my_schema',
		  xform_list            => V_XLST );
	</copy>
	
## 通用模型设置（简略方法）

[Oracle文档 45.7.12 CREATE_MODEL2 Procedure](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING.html#GUID-560517E9-646A-4C20-8814-63FDA763BFD9)

+ 模型名称	***model\_name***
	- 模型的名称，格式为\[ schema\_name \.\] model_name 。 如果未指定schema，则使用您自己的schema。 有关模型命名限制，请参见使用说明。
+ 功能 ***mining\_function***
	- 模型功能，见[功能 mining\_function](#功能mining_function)
+ 数据查询	***data_query***	
	- 包含模型训练数据的查询。
+ 设置列表	***set_list***	
	- 指定 SETTING_LIST，它是CLOB索引的表 VARCHAR2(30) ； 其中索引是设置名称，而CLOB是该名称的设置值。
+ Case\_Id列名称	***case\_id\_column\_name***
	- 构建数据中的案例标识符列。
+ 目标列名称	***target\_column\_name***
	- 对于监督模型，训练集中的目标列。 NULL 适用于无监督模型。
+ xform_list	***xform\_list***
	- 根据 PREP_AUTO 设置的值，在自动转换之外或代替自动转换而使用的转换列表 。

示例：

	<copy>
	DECLARE v_setlst DBMS_DATA_MINING.SETTING_LIST;
	DECLARE V_XLST DBMS_DATA_MINING_TRANSFORM.TRANSFORM_LIST;
	
	DBMS_DATA_MINING.CREATE_MODEL2 (
		 model_name            => 'my_model',
		 mining_function       => DBMS_DATA_MINING.REGRESSION,
		 data_query            => 'SELECT * FROM train_data',
		 set_list              => v_setlst,
		 case_id_column_name   => 'case_id',
		 target_column_name    => 'target',
		 xform_list            => V_XLST );
	</copy>
	
## 特殊模型设置

### 关联Association

| 	Setting Name	 | 	设定名称	 | 	默认值	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 	----	 | 
| 	ASSO\_MAX\_RULE\_LENGTH	 | 	<p style="width:8rem">最大规则长度</p>	 | 	<p style="width:1rem">4</p>	 | 	文本[2,20]	 | 	关联规则的最大规则长度。| 
| 	ASSO\_MIN\_CONFIDENCE	 | 	最低置信度	 | 	0.1	 | 	文本[0,1]	 | 	关联规则的最低置信度。 | 
| 	ASSO\_MIN\_SUPPORT	 | 	最低支持度	 | 	0.1	 | 	文本[0,1]	 | 	对关联规则的最低支持度。 | 
| 	ASSO\_MIN\_SUPPORT\_INT	 | 	最小绝对支持数	 | 	1	 | 	文本[0,1]	 | 	每个规则必须满足的最小绝对支持数。 该值必须是整数。 | 
| 	ASSO\_MIN\_REV\_CONFIDENCE	 | 	最小反向置信度	 | 	0	 | 	文本[0,1]	 | 	设置每个规则应满足的最小反向置信度。 规则的反向置信度定义为发生规则的事务数除以发生结果的事务数。整数形式，取值范围是0〜1。 | 
| 	ASSO\_IN\_RULES	 | 	包含商品列表	 | 	NULL	 | 	逗号分隔列表	 | 	设置适用于每个关联规则的包括规则：在指定的商品列表中，中至少一项必须作为先行或后续出现在关联规则中。 它是一个逗号分隔的字符串，包含包含项的列表。 如果未设置，则默认行为是不应用过滤。	 | 
| 	ASSO\_EX\_RULES	 | 	排除商品列表	 | 	NULL	 | 	逗号分隔列表	 | 	设置适用于每个关联规则的排除规则：在指定的商品列表中，这些项目均不能出现在关联规则中。 它是一个逗号分隔的字符串，其中包含排除项列表。 规则中不能包含任何项目。| 
| 	ASSO\_ANT\_IN\_RULES	 | 	先验包含商品列表	 | 	NULL	 | 	逗号分隔列表	 | 	设置先验的包含规则：在指定的商品列表中，其中至少一项必须出现在关联规则的先验商品中。 它是一个逗号分隔的字符串，包含包含项的列表。 每个规则的前一部分必须在列表中至少包含一项。| 
| 	ASSO\_ANT\_EX\_RULES	 | 	先验包含商品列表	 | 	NULL	 | 	逗号分隔列表	 | 	设置先验的排除规则：在指定的商品列表中，这些项目均不能出现在关联规则的先验部分中。 它是一个逗号分隔的字符串，其中包含排除项列表。 规则的前一部分中不能包含任何项。 | 
| 	ASSO\_CONS\_IN\_RULES	 | 	后验包含商品列表	 | 	NULL	 | 	逗号分隔列表	 | 	设置后验的包括规则：在指定的商品列表中，其中至少一个项目必须出现在关联规则的后验部分中。 它是一个逗号分隔的字符串，包含包含项的列表。 每个规则的结果必须是列表中的一项。 | 
| 	ASSO\_CONS\_EX\_RULES	 | 	后验排除商品列表	 | 	NULL	 | 	逗号分隔列表	 | 	设置后验的排除规则：在指定的商品列表中，这些项目均不能出现在关联规则的后验部分中。 它是一个逗号分隔的字符串，其中包含排除项列表。 因此，任何规则都不能在列表中包含任何项目。 排除规则可用于减少必须存储的数据，但可能要求用户构建额外的模型以执行不同的包含或排除规则。 | 
| 	ASSO\_AGGREGATES	 | 	聚合列	 | 	NULL	 | 	逗号分隔列表	 | 	指定要聚合的列。  ITEM\_VALUE 不是必需值。它是一个逗号分隔的字符串，其中包含要聚合的列的名称。 列表中的列数必须小于等于10。 如果DMMS\_ITEM\_ID\_COLUMN\_NAME 设置为指示事务输入数据，您可以设置 ASSO\_AGGREGATES 。 请参阅 DBMS\_DATA\_MINING-全局设置 。 数据表必须具有有效的列名，例如 ITEM\_ID 和 CASE\_ID ，分别来自 ODMS\_ITEM\_ID\_COLUMN\_NAME 和 case\_id\_column\_name 。对于每个项目，用户可以提供几列进行汇总。 它需要更多的内存来缓冲多余的数据。| 
| 	ASSO\_ABS\_ERROR	 | 	采样绝对误差	 | 	0.5 * MAX( ASSO\_MIN\_SUPPORT, ASSO\_MIN\_CONFIDENCE )  | 	(0,MAX(最小支持度, 最小置信度)]	 | 	指定关联规则采样的绝对误差。 较小的值 ASSO\_ABS\_ERROR 可获得较大的样本量，这将给出准确的结果，但需要更长的计算时间。 设置一个合理的值 ASSO\_ABS\_ERROR ，例如其默认值，以避免大样本量。 | 
| 	ASSO\_CONF\_LEVEL	 | 	样本置信度	 | 	0.95	 | 	[0,1]	 | 	指定关联规则样本的置信度。 值越大， ASSO\_CONF\_LEVEL 获得的样本量越大。 之间的任何值 0.9 ，并 1 是合适的。| 

### 分类Classification

| 	Setting Name	 | 	设定名称	 | 	默认值	 | 	设定值	 | 	描述	 | 
| 	----	 | 	----	 | 	----	 | 	----	 | 	----	 | 
| 	CLAS\_COST\_TABLE\_NAME	 | 	<p style="width:8rem">成本矩阵表</p>	 | 		 | 	表名称	 | 	（仅决策树）表的名称，该表存储算法在构建模型时将使用的成本矩阵。 成本矩阵指定了与错误分类相关的成本。 只有决策树模型才能在构建时使用成本矩阵。 所有分类算法都可以在应用时使用成本矩阵。 成本矩阵表是用户创建的。 有关 列要求， 请参见 “ ADD\_COST\_MATRIX过程 ” 。 有关成本的信息， 请参见 Oracle数据挖掘概念 。	 | 
| 	CLAS\_PRIORS\_TABLE\_NAME	 | 	先验概率表	 | 		 | 	表名称	 | 	（朴素贝叶斯）表的名称，该表存储先验概率以抵消构建数据和评分数据之间分布的差异。 先验表是用户创建的。 有关 列要求， 请参见《 Oracle Data Mining用户指南》 。 有关先验的其他信息， 请参见 Oracle数据挖掘概念 。	 | 
| 	CLAS\_WEIGHTS\_TABLE\_NAME	 | 	权重表	 | 		 | 	表名称	 | 	（仅GLM和SVM）在SVM分类和GLM Logistic回归模型中存储各个目标值的加权信息的表的名称。 该算法使用权重来偏向模型，以偏向于更高的权重类别。 类权重表是用户创建的。 有关列要求， 请参见《 Oracle Data Mining用户指南》 。 有关类权重的其他信息， 请参见 Oracle Data Mining Concepts 。	 | 
| 	CLAS\_WEIGHTS\_BALANCED	 | 	类均衡	 | 	OFF	 | 	ON;OFF	 | 	此设置表明算法必须创建一个平衡目标分布的模型。 在存在稀有目标的情况下，此设置最相关，因为平衡分布可以实现更好的平均准确度（每类准确度的平均值），而不是总体准确度（有利于优势等级）。| 
| 	CLUS\_NUM\_CLUSTERS	 | 	最大叶簇数	 | 	10	 | 	文本 >=1	 | 	聚类算法生成的最大叶簇数。 该算法可能会返回较少的群集，具体取决于数据。 增强的 k -均值通常会产生由所指定的确切数目的簇，除非有更少的不同数据点。 期望最大化（EM）根据数据指定的数量 ，可能返回的群集少于该指标。 EM返回的簇数不能大于组件数，这由特定于算法的设置控制。 （请参阅 “学习的期望最大化设置” 表）根据这些设置，群集可能少于组件。 如果禁用了组件群集，则群集数等于组件数。 对于EM，默认值该设置是系统确定的。 | 
| 	FEAT\_NUM\_FEATURES	 | 	特征数量	 | 		 | 	文本 >=1	 | 	特征提取模型要提取的特征数量。 默认值是由算法根据数据估算得出的。 如果矩阵的秩小于此数字，则将返回较少的特征。 对于CUR矩阵分解，该 FEAT\_NUM\_FEATURES 值与该 CURS\_SVD\_RANK 值相同 。	 | 