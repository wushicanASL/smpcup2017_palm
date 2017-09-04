# SMPCUP 2017
Team: SEU palm lab
Team Members: Zhikai Zhang, Lei Miao
Task1 : Keywords Extraction
It can be seen as a classification problem. Firstly, candidate keywords are extracted; Secondly, features of these candidates are generated; Finally, classification can be conducted via a classifier, here, we use a simple 3-layer neutral network. 
Task2 : User Interests Mining
It can be seen as a classification/ranking problem. Given users' documents, we have to rank the 42 predefined interest tags and the top 3 are predicted as a user's interests. The users' documents can be classified into several types, i.e., post, browse, comment, vote-up, favorite. The vote-down type is ignored as it's less important. We generate the feature of a specific user by accumulating the document-level features over the user's documents. The features are generated by lda and doc2vec, and lda features seem to be more important which fits our expectations. Then we use a 3-layer neutral network for classification.
Task3 : User Vitality Prediction
It can be seen as a regression problem. Considering the features over time, LSTM or GRU can be used in this task.

# results

|               | task1         | task2         | task3         |
|:-------------:|:-------------:|:-------------:|:-------------:|
|validation set | 0.6216        | 0.4712        |0.7475         |
|test set       | 0.6046        | 0.4579        |0.7495         |

# code description of task1 and task2
基于目录结构的描述
d2v文件夹
预训练的doc2vec模型。[百度网盘链接](https://pan.baidu.com/s/1jHFKzxc)

data文件夹
存放训练集、验证集、测试集以及文档数据，data/seg_data/中存放已分好词的文档数据，一个文档放到一个文件下，里边放了一个样例文件。

dicts文件夹
存放字典文件。

lda文件夹
预训练的lda模型。

model
存放task1、task2的神经网络模型文件。

d2v_model.py
定义了类Doc2Vec_Model，加载预训练的doc2vec模型。

doc_preprocess.py
读取词典，封装两个处理词的列表的函数。

lda_model.py
定义了类LDA_Model，加载预训练的lda模型。

task1_main.py
执行task1。

task1_nn_keyword_classifier.py
基于神经网络的关键词分类器，以及特征提取器。

task1_nn_keyword_classifier_validation.py
训练神经网络分类器并验证结果。

task1_tfidf_keyword_extractor.py
基于tfidf与规则的关键词提取，用于提取候选关键词。

task2_main.py
训练模型，保存模型，验证，执行task2。

util.py
工具类，用于提供读取训练集、验证集、测试集以及其他文件的基本操作。

其他说明
doc2vec模型使用python gensim实现。
lda模型使用微软的lightlda实现，目录下pickle开头的文件是lda模型的输出经过再处理得到的文件。例如pickle.doc_spec_z_dist，该文件存储着一个dict类型，key为文档的id，值是一个dict类型（键为topic，值为计数），从而保存了每个文档的主题分布。
运行环境：
windows， python版本3.4.4。
python package版本：
gensim (2.2.0)
Keras (2.0.6)
numpy (1.12.1+mkl)
scikit-learn (0.18.1)
scipy (0.19.0)
Theano (0.8.1)
pickleshare(0.6)

# code description of task3
任务3的程序包含两部分：（1）gen_feature（2）train_and_predict
（1）	gen_feature：
程序组成：task3_preprocess.py
运行环境：windows，python2.7
程序说明：在task3_preprocess.py指定的base_path路径下放置原始数据集，运行task3_preprocess.py自动提取信息生成训练集、验证集和测试集的频率特征。
（2）	train_and_predict：
程序组成：GRU.py CallBackMy.py
运行环境：ubuntu，python2.7，keras1.2
程序说明：CallBackMy.py定义模型的回调函数用于中间结果检测，GRU.py是主程序用于训练回归模型并生成预测结果。在GRU.py指定的data_path路径下放置特征数据，运行主程序GRU.py训练RNN模型直接生成验证集和测试集的预测结果。data文件夹是task3的数据样例。根目录下是特征文件。Epochs_result是模型的中间结果，Model是模型的参数记录，result是最终的预测结果。

