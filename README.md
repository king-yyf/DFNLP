
# DFNLP #


DF natural language processing system  


实习期间做的NLP的基础工具，包括中文分词、词性标注和命名实体识别功能,使用的方法是基于词典、规则加CRF统计的方法，仅使用pku的数据训练了一个crf模型，由于pku分词数据和msr粒度和规范不太相同，所以用pku数据训练的模型在msr数据上表现一般。  


词性标注使用的是人民日报的标注规范。

	名词         n  
                nr 人名  
                ns 地名  
                nt 机构团体  
                nz 其它专名  
     时间词      t  
     处所词      s  
     方位词      f  
     动词        v  
                vd 副动词  
                vn 名动词  
     形容词      a  
                ad 副形词  
                an 名形词
     区别词      b  
     状态词      z  
     代词       r  
     数词       m  
     量词       q  
     副词       d  
     介词       p  
     连词       c  
     助词       u  
     叹词       e  
     语气词      y  
     拟声词      o  
     简称略语    j  
     习用语      l  
     成语       i  
     前接成分    h  
     后接成分    k  
     连接语    无  
     语素字      g  
                Ag 形语素  
                Dg 副语素  
                Ng 名语素  
                Tg 时语素  
                Vg 动语素  
     非语素字    x  
     标点符号    w  


命名实体识别识别五种类型的实体，分别为：

`Organization   //组织机构`  
`Datetime      //时间，日期`  
`Location     //地名,包括城市，国家等`  
`Person        //人名`  
`Number         //数字`  
`Basic          //其他实体，默认初始为basic`  


## 依赖软件
* g++ (version >= 4.1 is recommended ) or clang++(Apple LLVM version 9.0.0);
* cmake (version >= 2.8 is recommended );

## 下载和编译

1.下载crfpp软件包，放到工程主目录下
2.安装crf++，
% ./configure 
% make
% su
%make install (可选)
3.cd build
4.cmake ..
5.make

## 使用方法
*make后生成dfnlp二进制程序，运行 :

./dfnlp

## 结果示例
* input = “吴学研，男，1983年9月23日出生，汉族，无职业，住哈尔滨市松北区。”
* output:

1. 分词结果  
----------- cut result:  
吴学研/ ，/ 男/ ，/ 1983年/ 9月/ 23日出生/ ，/ 汉族/ ，/ 无职业/ ，/ 住/ 哈尔滨市/ 松北区/ 。/   

2. 词性标注结果  
----------- pos result:  
<吴学研, n> <，, n> <男, b> <，, n> <1983年, t> <9月, t> <23日出生, v> <，, nz> <汉族, nz> <，, n> <无职业, n> <，, n> <住, v> <哈尔滨市, ns> <松北区, ns> <。, w>。

3. 命名实体识别结果  
----------- ner result:  
word : 吴学研    pos : n    begin_pos : 0    key_type : Person  
word : ，    pos : n    begin_pos : 3    key_type : Basic  
word : 男    pos : b    begin_pos : 4    key_type : Basic  
word : ，    pos : n    begin_pos : 5    key_type : Basic  
word : 1983年9月    pos : v    begin_pos : 6    key_type : Datetime  
word : 23日出生    pos : v    begin_pos : 13    key_type : Basic  
word : ，    pos : nz    begin_pos : 18    key_type : Basic  
word : 汉族    pos : nz    begin_pos : 19    key_type : Basic  
word : ，    pos : n    begin_pos : 21    key_type : Basic  
word : 无职业    pos : n    begin_pos : 22    key_type : Basic  
word : ，    pos : n    begin_pos : 25    key_type : Basic    
word : 住    pos : v    begin_pos : 26    key_type : Basic  
word : 哈尔滨市松北区    pos : w    begin_pos : 27    key_type : Location  
word : 。    pos : w    begin_pos : 34    key_type : Basic  



## 分词性能评测
根据第二届国际汉语分词测评（The Second International Chinese Word Segmentation Bakeoff)发布的国际中文分词测评标准，将DFNLP分别与国内常见的分词软件分别在pku_test(510KB), msr_test(560KB)数据集上进行测试。使用icwb2-data 提供的perl脚本计算分词结果的召回率和准确率，其中ICTCLAS，THULAC_lite，LTP-3.2.0的结果数据使用的是清华THULAC提供的结果数据和运行时间，我仅测试了jieba（c++）和DFNLP的性能和时间，然后以jieba为基准，将DFNLP的运行时间换算为新的时间，例如：例如THULAC页面测试的jieba时间为0.4s，而我测试时jieba和DFNLP分别为0.2s，0.3时，那么如果jieba为0.4s时，DFNLP将换算为0.6s（不严谨的列了一下），准确率、召回率方面结果应该是相同的，测试结果如下：

## pku_test(510KB)

|  Algorithm  |  Precision   |   Recall   |  F-Measure  |    Time     |
|-------------| :----------: | :--------: | :---------: | :---------: |
|   ICTCLAS   |    0.939     |   0.944    |    0.941    |    0.53s    |
|  jieba(C++) |	   0.850	 |   0.784	  |    0.816	|    0.23s    |
| THULAC_lite |	   0.944	 |	 0.908    |	   0.926	|    0.51s    |
|  LTP-3.2.0  |	   0.960	 |   0.947    |	   0.953    |	 3.83s    |
|    DFNLP    |    0.945	 |   0.941    |	   0.943	|    0.77s    |



## msr_test(560KB)

|  Algorithm  |  Precision   |   Recall   |  F-Measure  |    Time     |
|-------------| :----------: | :--------: | :---------: | :---------: |
|   ICTCLAS   |    0.869     |   0.914    |    0.891    |    0.55s    |
|  jieba(C++) |	   0.814	 |   0.809	  |    0.811	|    0.26s    |
| THULAC_lite |	   0.877	 |	 0.899    |	   0.888	|    0.62s    |
|  LTP-3.2.0  |	   0.867	 |   0.896    |	   0.881    |	 3.21s    |
|    DFNLP    |    0.833	 |   0.857    |	   0.845	|    0.63s    |




