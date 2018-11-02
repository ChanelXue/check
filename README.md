联合学习:blush:
===========================
Bi-LSTM-Bi-TreeLSTM
------
Bi-LSTM-Bi-SeqLSTM
------
	
|Author|16S051014|
|---|---

## 环境配置:
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
## 使用方法:
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
	_修改onlyner.yaml文件重新测试seq的test数据,predictionExtension:ann_
	build/relation/RelationExtraction_ner --test -y yaml/parameter-onlyner.yaml
	build/relation/RelationExtraction_pip --test -y yaml/parameter-seqpip.yaml
	python evaluation_split.py 
##### sequence_joint:
	nohup build/relation/RelationExtraction_seqjoint --train -y yaml/parameter-seqjoint.yaml > myout-seqjoint.file 2>&1 &
	build/relation/RelationExtraction_seqjoint --test -y yaml/parameter-seqjoint.yaml
	python evaluation_joint.py 
	python NER_evaluation_joint.py 
##### eg:
	nohup build/relation/RelationExtraction_seqjoint+0.6-0.4 --train -y yaml/parameter-check.yaml > myout-seqjoint+0.6-0.4.file 2>&1 &
	

Bi-LSTM-CRF-Bi-SeqLSTM
------
## 环境配置:
	python 2.7+
	python 3
	tensorflow
***
## 使用方法:
### 1、处理语料文件
	cd data/data_generation/run
	_运行zsh dnnfeature_extract.sh，通过查看log_dnnfeatures_extract.log看运行是否成功_
	zsh dnnfeature_extract.sh ../../ChineseDate/valid ../data_out/valid/
	zsh dnnfeature_extract.sh ../../ChineseDate/test ../data_out/test/
	zsh dnnfeature_extract.sh ../../ChineseDate/train ../data_out/train/
	_在src/getBIOTagForNER.py添加约束_
### 2、移动文件
_重命名到 模型/data/attribute_data/raw_model_data
### 3、生成特征dict
	cd data_generation/src
	python fea_process.py
### 4、run models	
##### eg:
	cd lstm_crf_constriant/src_bilstm_crf_no_pretrian/
	nohup python run.py > myout.file 2>&1 &
	
BIOHD1234
------	
## 使用方法:
### 1、将数据转换为BIOHD1234

	cd data/data_generation/run_BIOHD_Multi
	_运行zsh dnnfeature_extract.sh，通过查看log_dnnfeatures_extract.log看运行是否成功_
	zsh dnnfeature_extract.sh ../../ChineseDate/valid ../data_out/BIOHD/valid/
	zsh dnnfeature_extract.sh ../../ChineseDate/test ../data_out/BIOHD/test/
	zsh dnnfeature_extract.sh ../../ChineseDate/train ../data_out/BIOHD/train/
### 2、移动文件
_重命名到 模型/data/attribute_data/raw_model_data
### 3、生成特征dict
### 4、share文件整理
_map、goldresult、pos、infor.txt等_
### 5、run models
##### eg:
	cd BIOHD/src_bilstm_crf_BIOHD/
	nohup python run.py > myout.file 2>&1 &
Multi-Label
------	
## 使用方法:
### 1、将数据转换为Multi-Label
	cd data/data_generation/run_BIOHD_Multi
	_运行zsh dnnfeature_extract.sh，通过查看log_dnnfeatures_extract.log看运行是否成功_
	zsh dnnfeature_extract.sh ../../ChineseDate/valid ../data_out/Multi-Label/valid/
	zsh dnnfeature_extract.sh ../../ChineseDate/test ../data_out/Multi-Label/test/
	zsh dnnfeature_extract.sh ../../ChineseDate/train ../data_out/Multi-Label/train/
### 2、移动文件
_重命名到 模型/data/attribute_data/raw_model_data
### 3、生成特征dict
### 4、share文件整理
_map、goldresult、pos、infor.txt等_
### 5、run models
##### eg:
	cd Multi-Label/src_bilstm_crf_MultiLabel/
	nohup python run.py > myout.file 2>&1 &
