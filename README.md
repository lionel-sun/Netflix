# Netflix 电影评分推荐
[数据集](https://www.kaggle.com/netflix-inc/netflix-prize-data "kaggle 链接")
## 数据预处理
- 将数据从txt文件导入DF中并且做格式转换
- 将四个DF的数据合并作为完成的数据集
- 将probe中的数据从数据集剔除作为测试集
- 对数据集进行压缩（由于内存不够用，全量数据CSV2.57G无法导入内存进行运算），然后保存到本地并且清理内存。对电影和用户分别取(m, n)的倍数id。
- 数据集直接通过文件导入，然后做kfold交叉验证
## 模型训练
分别使用以下四个模型：

#### 1，BaselineOnly
- （4，4）压缩后做kfold验证RMSE：0.9257, probe的RMSE:0.9862
- （2，2）压缩后全集作为训练集，probe的RMSE:0.9800

#### 2，FunkSVD
- （4，4）压缩后做kfold验证RMSE：0.9043, probe的RMSE:1.0928
- （2，2）压缩后全集作为训练集，probe的RMSE:1.0539

#### 3，biasSVD
- （4，4）压缩后做kfold验证RMSE：0.8921, probe的RMSE:0.9817
- （2，2）压缩后全集作为训练集，probe的RMSE:0.9658

#### 4，SVD++
- （4，4）压缩后做kfold验证RMSE：0.9053
- （2，2）压缩后全集作为训练集，probe的RMSE:

## 调整超参

[baselineonly参数说明](https://surprise.readthedocs.io/en/stable/prediction_algorithms.html#baseline-estimates-configuration "官方文档说明")
[SVD参数说明](https://surprise.readthedocs.io/en/stable/matrix_factorization.html#surprise.prediction_algorithms.matrix_factorization.SVD "官方文档说明")
[SVD++参数说明](https://surprise.readthedocs.io/en/stable/matrix_factorization.html#surprise.prediction_algorithms.matrix_factorization.SVDpp "官方文档说明")

使用GridSearchCV进行超参数调整。
大概设计参数范围：
param_grid = {'n_epochs': [5, 10], 'lr_all': [0.002, 0.005],
              'reg_all': [0.4, 0.6]}
