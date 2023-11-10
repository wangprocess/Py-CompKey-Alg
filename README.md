Py-CompKey-dev

竞争性关键词的搜索引擎开发

第一阶段：
	完成数据的预处理和清洗工作
	根据关键词的出现频率来选取种子关键词
	通过种子关键词来获取中介关键词
	解决了文件的编码获取问题
	完成了代码的优化处理
	进行了分词工具的横向对比
	
第二阶段：
	首先，在第一步的获取中介关键词的基础上，进行sa/s的计算，算出中介关键词的权重
	然后，进行备选竞争关键词的提取，使用"提取搜索信息-jieba分词-停语词清洗-统计出现次数“的方式,将一些竞争性关键词存起来
	接下来，计算每一个竞争性关键词的comp竞度，这里的comp度是在中介关键词a的基础上的comp，不是最后的comp
	最后，计算总的comp度，将每一个竞争性关键词在a上的comp乘上a的权重，求和后得出总的comp度，将其按从大到小排序，取top5出来
	基于上述的计算，将竞争性关键词的comp度和中介关键词的权重进行绘图，使用plot绘出
	
	创新点：
		- 从机器学习中得到灵感，进行权重的平滑，降低突发式搜索的影响（2016问题）公式： alpha * avg(weight) + (1 - alpha) * weight
		- 针对种子关键词和竞争性关键词不同时出现在中介关键词中，或者说种子关键词、中介关键词、竞争关键词不能互相包含，基于此，
		  进行了词义相似度的判断，使用spacy进行判断，也就是不仅不互相包含，也不十分相似；在此过程中探究了word2vec词向量融入和纯使用spacy的区别，由于时间选择了spacy
		- 还探究了在公式的带入计算时，搜索量要统计的是词在一句话中出现就算一次，还是jieba分词后，词的出现次数
		- 还发现了一个问题，就是即使在已经剔除了中介关键词和竞争性关键词同时出现的情况下，还会出现，在最后的竞争性关键词列表中，
		  会有中介关键词的出现，这是因为关联搜索的问题，也就是某一个中介关键词和另一个中介关键词经常联合搜索，会导致在中介关键词A下，
		  中介关键词B的comp度比较大，最后出现在总的comp的前列，这不是没有剔除不同时出现的情况，而是特殊的情况，所以应该剔除