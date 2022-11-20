# 一、说明
## 1.1．数据集说明：数据集是由8个同结构的csv数据文件组成的北京租房数据。
## 1.2．问题说明：什么因素影响房子的租金
# 二、导入并合并数据文件
    import pandas as pd

    data = pd.DataFrame([])
    for i in range(1,9):
        path = r'./bj_danke_'+str(i)+'.csv'
        data = pd.concat([data,pd.read_csv(path)],axis=0) 
    #print(data.shape)
    data.head()
![image](https://user-images.githubusercontent.com/106458142/202876630-a84ae106-31aa-414c-bda3-baac8999a07b.png)

# 三、数据预处理
    import re

    #2.1 删除属性为空的行
    data = data.dropna(axis=0, how='any')
    # print(data.shape)

    #2.2 以‘编号’为行索引，故编号唯一，需要删除重复值
    data = data.drop_duplicates(subset = ['编号'])
    # print(data.shape)
    data.set_index('编号',inplace=True)

    #2.3 新增一列‘小区与地铁站的距离’
    data.loc[:,'小区与地铁站的距离'] = data['地铁'].apply(lambda x : int(re.search('(\d{1,})米',x)[1]) if re.search('(\d{1,})米',x) else None)

    #2.4 新增一列‘楼的总层数’
    data.loc[:,'楼的总层数'] = data['楼层'].apply(lambda x : int(re.search('(\d{1,})/(\d{1,})层',x)[2]) if re.search('(\d{1,})/(\d{1,})层',x) else None)

    #2.5 新增一列‘所在楼层’
    data.loc[:,'所在楼层'] = data['楼层'].apply(lambda x : int(re.search('(\d{1,})/(\d{1,})层',x)[1]) if re.search('(\d{1,})/(\d{1,})层',x)else None)

    #2.6 删除属性值为空的行
    data = data.dropna(axis = 0 ,how = 'any')
    # print(data.shape)

    #2.7 新增一列‘房子每平方的价格’
    data['价格'] = data['价格'].astype('f8')
    data['面积'] = data['面积'].astype('f8')
    data.loc[:,'房子每平方的价格'] = data['价格']/data['面积']

    data.head()
![image](https://user-images.githubusercontent.com/106458142/202876653-f10a65cf-71c3-48af-aee5-35560383d9f9.png)
# 四、数据分析
## 4.1 小区总体分析
### 4.1.1 各区租房每平米的平均价格比较分析
如下图所示，北京各个区的租房每平米的平均价格不同，这也反映出每个区经济发展水平的关系。每平米的平均价格越高在一定程度上说明该区的租房需求越旺盛，人口流动越大。故由于每个区经济发展水平的差异以及租房需求量的不同影响了各个区租房价格。
![3_1_1](https://user-images.githubusercontent.com/106458142/202876691-59313357-05d5-465c-a355-21a7f5ee8e30.jpg)
### 4.1.2 各区老旧小区或小民房情况分析
将楼层小于7的楼房认为是老旧小区或小民房，可以发现各区房源中老旧小区或小民房占比较低
![3_1_2](https://user-images.githubusercontent.com/106458142/202876746-c33cc049-d0cb-414b-8c76-44ca223d83ce.jpg)
### 4.1.3 各区房子价格与地铁站距离、户型类别的关系分析
由图可知，与地铁站同等的距离条件下，中户型的每平方的租金与小户型的相比更低，并且市场上有大量的小户型的房源而中大户型的不多。根据有限数据推断有两种情况，其一则是若中大户型的总房源数与现有的相差不多，说明中大户型的市场需求非常低，即使每平方的租金很低，但总租金比小户型的要高，由于一般情况房子合租的人数不会太多，所以导致中大户型出租困难；其二则是若中大户型的总房源数多，说明中大户型的市场需求旺盛，因为与小户型相比，价格上中大户型更适合多人合租以及舒适度上中大户型的更好。
![3_1_3](https://user-images.githubusercontent.com/106458142/202876758-23af83e5-d940-4288-bf3f-b1d8540fa3af.jpg)
## 4.2 地理环境分析
### 4.2.1 房子所在楼层与价格的关系分析
如图所示，小于7层的房源数比其他的多并且该房子每平方米的价格比其他的低，说明租客偏爱有电梯(层数大于等于7)或楼层高的房子。
![3_2_1](https://user-images.githubusercontent.com/106458142/202876781-c2df13ae-1b56-4b13-abcf-ff7dd4fbca21.jpg)
### 4.2.2 各区房子价格与地铁站距离、有无电梯的关系分析
如图所示，在小区与地铁站相同距离的条件下，有电梯的房子每平米的平均价格一般都比无电梯的房子每平米的平均价格要高，说明有无电梯是影响房子租金的一个重要因素。
![3_2_2](https://user-images.githubusercontent.com/106458142/202876791-c8fc875b-b45b-4c7b-9e21-0a6278e30e10.jpg)
