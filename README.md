# 一、说明
## 1.1．数据集说明：数据集是由8个同结构的csv数据文件组成的北京租房数据。
## 1.2．问题说明：什么因素影响房子的租金
# 二、导入并合并数据文件
import pandas as pd

data = pd.DataFrame([])
for i in range(1,9):
    path = r'./bj_danke_'+str(i)+'.csv'
    data = pd.concat([data,pd.read_csv(path)],axis=0) 
# print(data.shape)
data.head()
# 三、数据预处理
