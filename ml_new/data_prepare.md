# 数据准备和转换

## Introduce

## 数据准备

准备数据进行挖掘的第一步是创建一个 案例表。如果所有数据都驻留在单个表中，并且每种情况（记录）的所有信息都包含在单个行（单记录情况）中，则已经完成了此过程。
如果数据驻留在多个表中，则创建数据源将涉及视图的创建。为了简单起见，术语“案例表”在这里用于表示表或视图。

1. **创建嵌套列**

	[Oracle文档  Using Nested Data](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmprg/using-nested-dita.html#GUID-4BBB376F-7CED-4E23-826B-922D00CBFA44)
	
	当数据源包含事务处理数据（多记录案例）时，必须在嵌套列中将事务聚合到案例级别。在事务数据中，每种情况的信息都包含在多行中。一个示例是在产品级别进行挖掘时采用星型模式的销售数据。单个产品（案例）的销售量存储在多行中，因为一段时间后该产品在许多商店中都卖给了许多客户。

	注意：O-Cluster是唯一不支持嵌套数据的算法。

	Oracle Data Mining支持以下嵌套对象类型：

	- DM\_NESTED\_BINARY_DOUBLES
	- DM\_NESTED\_BINARY_FLOATS
	- DM\_NESTED\_NUMERICALS
	- DM\_NESTED\_CATEGORICALS

2. **转换列数据类型**

	您必须将 列的数据类型 （如果其类型导致Oracle Data Mining错误解释）。例如，邮政编码标识不同的邮政区。他们并不表示顺序。如果邮政编码存储在数字列中，则将其解释为数字属性。您必须转换数据类型，以便模型可以将列数据用作分类属性。您可以使用TO_CHAR转换数字，用LPAD函数保留前导0来执行此操作。
	
	<copy>
		LPAD(TO_CHAR(ZIPCODE),5,'0')
	</copy>

3. **文本转换**

	您可以使用Oracle数据挖掘来挖掘文本。一旦案例表中的文本列经过适当的转换，就可以对其进行挖掘。

	文本列必须位于表中，而不是视图中。转换过程使用了Oracle Text的几个功能。它将表格每一行中的文本视为一个单独的文档。每个文档都转换为一组称为术语的文本标记，这些标记具有数字值和文本标签。文本列将转换为的嵌套列DM\_NESTED\_NUMERICALS。
	
4. **自定义转换**
	一些转换是由业务问题的定义决定的。例如，您想构建一个模型来预测高收入客户。由于您当前客户的收入数据以美元为单位，因此您需要定义“高收入”的含义。使用您从过去的经验中得出的一些公式，可以在构建模型之前将收入属性重新编码为低，中和高范围。

	另一种常见的业务转换是将日期信息转换为经过的时间。例如，可以将出生日期转换为年龄。

## 自动数据准备(ADP)

[Oracle文档  Understanding Automatic Data Preparation](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmprg/automatic-data-preparation.html#GUID-C8F33CF2-3A13-4A99-AB2D-13649784C8B4)

大多数算法都需要某种形式的数据转换。在模型构建过程中，自动数据准备(ADP，Automatic Data Preparation）自动执行算法所需的转换。在计算自动转换时，Oracle Data Mining使用启发式方法来满足给定算法的常见要求。在大多数情况下，此过程可产生合理的模型质量。

您可以选择用自己的其他转换来补充自动转换，也可以选择自己管理所有转换。

1. **分箱**

	分箱（也称为离散化）是一种将连续数据离散化的技术，将相关值分组在一起，以减少不同值的数量。

	分级可以极大地提高资源利用率，并在不显着降低模型质量的情况下缩短模型构建的时间。分箱可以通过加强属性之间的关系来提高模型质量。
	
	监督分箱是智能分箱的一种形式，其中数据的重要特征用于确定分箱边界。在有监督的分箱中，分箱边界由一个单预测决策树确定，该决策树考虑了与目标的联合分布。有监督的分箱可以用于数字和分类属性。

2. **规范化**

	规范化是减少数值数据范围的最常用技术。大多数规范化方法将单个变量的范围映射到另一个范围（通常为0,1）。

	ADP不影响的算法：	
	- *Apriori*
	- *决策树*
	
	数值属性会标准化的算法：
	- *广义线性回归*
	- *k均值*
	- *非负矩阵分解*
	- *奇异值分解*
	- *支持向量机*

	用高斯分布建模的单列（非嵌套）数字列被标准化。ADP对其他类型的列没有影响：
	- *期望最大化*

	所有属性都通过有监督的分箱方法进行分箱：		
	- *最小描述长度*
	- *朴素贝叶斯*
	
	数值属性通过等宽合并的一种特殊形式进行合并，该等宽合并会自动计算每个属性的合并数量。具有所有空值或单个值的数字列将被删除：
	- *O集群*

## 转换列表

**关于转换列表**

[Oracle文档  Embedding Transformations in a Model](https://docs.oracle.com/en/database/oracle/oracle-database/19/dmprg/embedding-transformations-model.html#GUID-1949B958-6D98-4D00-BF9D-5AC3D8620E11)

转换是一种SQL表达式，用于修改一个或多个列中的数据。数据通常必须经过某些转换才能用于构建模型。许多数据挖掘算法都有特定的转换要求。在对数据进行评分之前，必须以与训练数据转换相同的方式进行转换。

Oracle Data Mining支持自动数据准备（ADP），该功能可自动实现算法所需的转换。转换被嵌入到模型中，并在应用模型时自动执行。

如果需要其他转换，则可以将它们指定为SQL表达式，并在创建模型时提供它们作为输入。这些转换就像在ADP中一样被嵌入到模型中。

通过自动和嵌入式数据转换，可以为您处理大部分数据准备工作。您只需几个步骤即可创建模型并为多个数据集评分：

	· 确定要包括在案例表中的列。
	· 如果要包括事务数据，请创建嵌套的列。
	· 为ADP未处理的任何转换编写SQL表达式。
	· 创建模型，提供SQL表达式（如果指定）并标识包含文本数据的任何列。
	· 确保评分数据中的某些或所有列与用于训练模型的列具有相同的名称和类型。
	
1. 为属性指定转换指令

	转换列表定义为转换记录表。每个记录（transform_rec）指定属性的转换指令。

	 | 	字段	 | 	描述	 | 
	 | 	----	 | 	----	 | 
	 | 	attribute\_name</br>attribute\_subname | 	用于标识属性 | 
	 | 	expression	 | 	用于转换属性的SQL表达式。</br>例如，根据年龄区分成人和儿童：*CASE WHEN age < 19 THEN 'child' ELSE 'adult'*| 
	 | 	reverse_expression	 | 	用于反转转换的SQL表达式。</br>例如，使age属性的转换反转：*DECODE(age,'child','(-Inf,19)','[19,Inf)')*	 | 
	 | 	attribute_spec	 | 	指定属性的特殊处理，可以为null，也可以具有以下一个或多个值：</br> <strong><em>FORCE\_IN</em></strong> —仅对于广义线性模型有效，当启用ftr\_selection\_enable设置时，强制将属性包含在模型构建中。 （默认情况下禁用ftr\_selection\_enable。）。 无法为嵌套属性或文本指定FORCE\_IN。</br> ***NOPREP*** —当ADP启用时，阻止属性的自动转换。 如果未打开ADP，则此值无效。 您可以为嵌套属性指定NOPREP，但不能为嵌套属性中的单个子名称（行）指定NOPREP。</br> ***TEXT*** —表示属性包含非结构化文本。 ADP对此设置无效。 TEXT可以选择包含子设置POLICY\_NAME，TOKEN\_TYPE和MAX\_FEATURES。 | 

	示例：

		<copy>
			TYPE transform_rec IS RECORD (
			attribute_name      VARCHAR2(30),
			attribute_subname   VARCHAR2(4000),
			expression          EXPRESSION_REC,
			reverse_expression  EXPRESSION_REC,
			attribute_spec      VARCHAR2(4000));
		</copy>
	
2. 表达记录

3. 属性规范

4. 转换程序

	[Oracle文档  Summary of DBMS_DATA_MINING_TRANSFORM Subprograms](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_DATA_MINING_TRANSFORM.html#GUID-B08B61A5-CCD1-4C10-83DA-D3C30BD5587A)

	  | 	子程序	 | 	功能	 | 
	 | 	----	 | 	----	 | 
	 | 	CREATE\_BIN\_CAT	 | 	创建转换定义表以进行分类装箱	 | 
	 | 	CREATE\_BIN\_NUM	 | 	创建用于数字合并的转换定义表	 | 
	 | 	CREATE\_CLIP	 | 	创建用于剪切的转换定义表	 | 
	 | 	CREATE\_COL\_REM	 | 	创建用于删除列的转换定义表	 | 
	 | 	CREATE\_MISS\_CAT	 | 	创建用于定义缺失值处理的转换定义表	 | 
	 | 	CREATE\_MISS\_NUM	 | 	创建用于数字缺失值处理的转换定义表	 | 
	 | 	CREATE\_NORM\_LIN	 | 	创建用于线性归一化的转换定义表	 | 
	 | 	DESCRIBE\_STACK	 | 	描述转换列表	 | 
	 | 	GET\_EXPRESSION	 | 	VARCHAR2从转换表达式返回一个块	 | 
	 | 	INSERT\_AUTOBIN\_NUM\_EQWIDTH	 | 	在转换定义表中插入数字自动等宽度合并定义	 | 
	 | 	INSERT\_BIN\_CAT\_FREQ	 | 	在转换定义表中插入基于频率的分类合并定义	 | 
	 | 	INSERT\_BIN\_NUM\_EQWIDTH	 | 	在转换定义表中插入数字等宽度合并定义	 | 
	 | 	INSERT\_BIN\_NUM\_QTILE	 | 	在转换定义表中插入数字分位数合并表达式	 | 
	 | 	INSERT\_BIN\_SUPER	 | 	在数字和分类转换定义表中插入受监督的合并定义	 | 
	 | 	INSERT\_CLIP\_TRIM\_TAIL	 | 	在转换定义表中插入数字修剪定义	 | 
	 | 	INSERT\_CLIP\_WINSOR\_TAIL	 | 	在变换定义表中插入数字化Winsorizing定义	 | 
	 | 	INSERT\_MISS\_CAT\_MODE	 | 	在转换定义表中插入分类缺失值处理定义	 | 
	 | 	INSERT\_MISS\_NUM\_MEAN	 | 	在转换定义表中插入数值缺失值处理定义	 | 
	 | 	INSERT\_NORM\_LIN\_MINMAX	 | 	在转换定义表中插入线性最小值-最大值归一化定义	 | 
	 | 	INSERT\_NORM\_LIN\_SCALE	 | 	在转换定义表中插入线性比例尺标准化定义	 | 
	 | 	INSERT\_NORM\_LIN\_ZSCORE	 | 	在转换定义表中插入线性zscore规范化定义	 | 
	 | 	SET\_EXPRESSION	 | 	VARCHAR2向表达式添加一个块	 | 
	 | 	SET\_TRANSFORM	 | 	将转换记录添加到转换列表	 | 
	 | 	STACK\_BIN\_CAT	 | 	将分类合并表达式添加到转换列表	 | 
	 | 	STACK\_BIN\_NUM	 | 	将数字合并表达式添加到转换列表	 | 
	 | 	STACK\_CLIP	 | 	将裁剪表达式添加到转换列表	 | 
	 | 	STACK\_COL\_REM	 | 	将列删除表达式添加到转换列表	 | 
	 | 	STACK\_MISS\_CAT	 | 	将分类的缺失值处理表达式添加到转换列表中	 | 
	 | 	STACK\_MISS\_NUM	 | 	将数值缺失值处理表达式添加到转换列表中	 | 
	 | 	STACK\_NORM\_LIN	 | 	将线性归一化表达式添加到转换列表	 | 
	 | 	XFORM\_BIN\_CAT	 | 	使用分类装箱转换创建数据表的视图	 | 
	 | 	XFORM\_BIN\_NUM	 | 	使用数值合并转换创建数据表的视图	 | 
	 | 	XFORM\_CLIP	 | 	创建带有裁剪转换的数据表视图	 | 
	 | 	XFORM\_COL\_REM	 | 	创建具有列删除转换的数据表视图	 | 
	 | 	XFORM\_EXPR\_NUM	 | 	使用指定的数值转换创建数据表的视图	 | 
	 | 	XFORM\_EXPR\_STR	 | 	使用指定的分类转换创建数据表的视图	 | 
	 | 	XFORM\_MISS\_CAT	 | 	创建具有分类缺失值处理的数据表视图	 | 
	 | 	XFORM\_MISS\_NUM	 | 	创建具有数值缺失值处理的数据表视图	 | 
	 | 	XFORM\_NORM\_LIN	 | 	使用线性规范化转换创建数据表的视图	 | 
	 | 	XFORM\_STACK	 | 	创建转换列表的视图	 | 