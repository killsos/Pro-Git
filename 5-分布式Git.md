## 5-分布式Git

- 提交信息规则
	
	第一行不要超过50个字   准确描述变更
	
	第二行是空白行
	
	第三行开始更详细描述
	

- 信息模板

	简述信息
	
	空白行
	
	更详细信息
		
		- 1
		
		- 2
		
		- 3
		

- 为发布版打标签
	
	git tag -s v.15 -m "123"
	
	git push --tags
	

- 生成构建编号

	git describe 
	
	
- 准备发布

	git archive master --prefix='project/' | gzip > 'git describe master'.tar.gz
	
