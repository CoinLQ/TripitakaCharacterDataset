# 大藏经切字数据包
这是从大藏经经文图片中切分出的单个字的图片数据，每个月更新一个数据版本。现在的版本是1.3版，全部是数据来自高丽藏，未来会加入其他藏经的汉字数据。

百度云网盘共享链接: https://pan.baidu.com/s/1miJIuPq

## 格式说明
- segmentation_character.sql.zip 导出的字信息数据库表，数据库为postgresql，其中的部分字段说明如下：
  - char 对应的实际汉字
  - image 字图片文件的名称
  - page_id 字所在页面的ID
  - left, right, top, bottom 切分的字图片在经文图片中的坐标
  - is_correct 这个值由人工标注，表示字图片与字是否对应正确，值为1表示正确，值为-1表示不正确，值为0时表示没有经过人工标注。
  - accuracy 为程序标注的字对应的准确度，为0与1之间的一个浮点数，0表示最不准确，1表示最准确。这个值是以人工标注的正确和不正确的数据为样本，训练分类器预测得到的值，分类器当前使用的算法为逻辑回归。
- x00.tar x01.tar ...   字图片的打包文件，用一般的解压软件解压即可；每个字图片的路径为{page_id}/{image}
- 注：在1.3版的数据包中没有包含is_correct值为-1的数据，其中的数据包含is_correct为1或者is_correct为0且accuracy在0.85以上的数据。

## 字图片统计数据
请查看文件：字图片统计数据.txt

# 基于大藏经切字数据包的OCR识别引擎
## 基于LR（逻辑回归）。
通过人工检查，给每一个汉字挑选出至少50张正确图片和五十张错误图片作为初级样本，依据初级样本即可训练分类器。
## 基于深度学习
利用前面训练好的SVM分类器，给每一个汉字挑选出更多的正确图片，辅助人工抽查，来作为高级样本，依据高级样本来训练深度学习分类器。
