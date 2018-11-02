联合学习
===========================
Bi-LSTM-Bi-TreeLSTM
------
Bi-LSTM-Bi-SeqLSTM
------
	
|Author|SHI:blush:|
|---|---


# 环境配置:
	python 2.7+
	python 3
	Fedora Core 22
	clang++ 3.4
	boost 1.57
	yaml-cpp 0.5.1
	ICU4C 54.1

	tar xzf cnn.tar.gz
	tar xzf eigen.tar.gz
	mkdir build
	cd build
	cmake .. -DEIGEN3_INCLUDE_DIR=eigen -DCMAKE_CXX_COMPILER=/usr/lib/llvm-3.8/bin/clang++ 
	make
	cd ..
***
# 使用方法:
### 1、首先，需要提取训练集中的数据和测试集中的数据
	cd ChineseData/src 
	python get_1_to_n_datafile.py
### 2、提取特征，将数据转换为模型的输入形式
	cd joint-LSTM-ER/data/corpus
	zsh run.zsh
	zsh runseq.zsh
### 3、训练集和验证集划分
	python3 train_valid_split.py
### 4、修改yaml配置文件
	cd joint-LSTM-ER/yaml    parameter-check.yaml
### 5、run models
##### onlyner:
	nohup build/relation/RelationExtraction_onlyner --train -y yaml/parameter-onlyner.yaml > myout-onlyner.file 2>&1 &
	build/relation/RelationExtraction_onlyner --test -y yaml/parameter-onlyner.yaml
	python NER_evaluate.py 
##### regulation:
	python regulation_process.py
	python evaluate_regulation.py
##### sequence_pip:
	nohup build/relation/RelationExtraction_seqpip --train -y yaml/parameter-seqpip.yaml > myout-seqpip.file 2>&1 &
	cd data/corpus/corpus_seqpip/test
	find ./ -name "*.ann" | xargs rm -rf
	修改onlyner.yaml文件重新测试seq的test数据,predictionExtension:ann
	build/relation/RelationExtraction_ner --test -y yaml/parameter-onlyner.yaml
	build/relation/RelationExtraction_pip --test -y yaml/parameter-seqpip.yaml
	python evaluation_split.py 
##### sequence_joint:
	nohup build/relation/RelationExtraction_seqjoint --train -y yaml/parameter-seqjoint.yaml > myout-seqjoint.file 2>&1 &
	build/relation/RelationExtraction_seqjoint --test -y yaml/parameter-seqjoint.yaml
	python evaluation_joint.py 
	python NER_evaluation_joint.py 
##### eg:
	nohup build/relation/RelationExtraction_seqjoint+0.6-0.4 --train -y yaml/parameter-seqjoint+0.6-0.4.yaml > myout-seqjoint+0.6-0.4.file 2>&1 &
