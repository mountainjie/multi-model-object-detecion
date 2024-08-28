# multi-model-object-detecion
mobicom deadline schedule

## 朱永
* 写论文intro部分。
* 分别写出本地读ROI、预测ROI、真实label3种情况下的可视化代码，用于比较云端回传ROI的影响。
* 解决ROI云端回传八叉树下采样问题。（何伟博辅助部署）
* 下采样反馈机制，调节数据传输体量。（何伟博辅助部署）

* 进度与问题
<br>6月26日，无法利用图片在边端进行ROI确定，因为相机模型结构与八叉树结构的差异性。ROI的来源必须是云端的3D目标检测结果，应该从尽可能提高云端ROI的可靠性角度考虑问题。
<br>为了确定云端回传ROI的影响，需要伟博那边提供本地读取ROI的检测结果（loc+size+type），云端回传ROI的检测结果（loc+size+type），这个实验结果已经给过一部分了，需要把剩余的补齐。
<br>6月28日，解决云端回传ROI的情况下，检测算法出现过多误检测的问题。具体方案为，ROI信息投影到图像上，如果检测标签与语义分割标签不匹配，则丢弃检测ROI。（需要伟博配合提供以下数据，1、已经写好的点云投影代码，2、语义分割结果文件。）

## 何伟博

### 0828

当前问题主要是对于新出现的物体少检测而不是误检测

其他方法都要使用模型，边端算力不支持

1.八叉树采样和全局采样结合

2.多分辨率采样，对ROI区域用较高的分辨率和非ROI区域用较低的分辨率

3.先对2D图像进行分割,在云端利用分割结果生成虚拟点云，对原始场景补全，得到完整的目标进行3D检测。

![image](https://github.com/user-attachments/assets/5ed24ed7-dd50-461a-a5dd-311c98bdcccd)

1)首先对2D图像进行实例分割，得到分割结果

2)再将点云投影到2D图像上，每个实例上都会有一些投影的点云，如（b）所示，mask内的点云为黑色，其余的点云为灰色

3)根据前景区域内点云的深度信息生成虚拟点云，生成虚拟点的方法是在2D实例分割区域中进行随机采样K个点，根据周围点云的深度插值得到这K个点的深度值

4)最后再将这K个点投影到点云坐标系，得到虚拟点云，达到了对原始场景补全的目的，再进行3D检测

---

* 完成动态带宽条件变化的脚本，测试数据传输时延、检测帧率等参数，确保带宽条件被改变。
* 待完成

* 进度与问题

* 读取本地ROI，测试精度

![491371a6da2f63f1b351d1dcde5d300](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/24cf3709-002d-402a-b647-7224d5af6616)

使用上一帧预测结果作为ROI，测试精度

![1bbaa7da670b85fff974aab30740c89](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/a9987078-8beb-44cb-beee-0abff756d24a)

* 旧语义分割模型表现，半透明为旧语义分割模型预测结果，白色为大模型的分割结果

旧语义分割模型，对van和car的分辨不佳

![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/dc92b09b-ce36-464b-aa17-9691608d7eff)
![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/0dcdd309-d5af-46b5-8161-c4cb964b8464)
![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/139c4189-a682-44a0-8e9e-5a32c7cb33c1)
![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/aa227b0d-72a1-4218-a787-a07f13ed3c96)
![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/514f39ac-5c0b-4b38-a33d-a15169bb5532)

但大模型预测的cyclist，在远处的情况和与车重叠的情况也表现不佳

![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/f88c7d4b-cc68-4931-8360-8db0499ae5ec)
![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/9b8abdbc-631d-4e45-96b4-174ec9ae101c)

重叠时容易将前面的cyclist识别为障碍物

![image](https://github.com/mountainjie/multi-model-object-detecion/assets/75228656/9d652152-030b-4337-bf18-5a3bccda2ffd)



## 王涛
* 数据集构建
<br>对21个tracking数据集重新划分（0-15个tracking作为训练集，16到20个tracking作为测试集）
<br>1、纯点云训练数据集  （纯点云+标签）
<br>2、融合检测数据集    （对齐加强点云+标签）
<br>3、差帧融合数据集    （差帧加强点云+标签）  多个数据集，差1、2、3...帧
<br>4、预测融合数据集    （预测加强点云+标签）  多个数据集，预测1、2、3...帧 

* 精度测试实验
<br>1、纯点云推理精度测试（训练+推理）
<br>以方式1构建数据集并训练，分别画出每个损失迭代曲线和精度迭代曲线；在测试集上测试精度。
<br>2、融合推理精度测试  （训练+推理）
<br>以方式2构建数据集并训练，分别画出每个损失迭代曲线和精度迭代曲线。在测试集上测试精度
<br>3、差帧融合精度测试（仅推理）
<br>以方式3构建数据集，并用2中训练的模型在测试集上进行测试。
<br>4、预测融合精度测试（仅推理）
<br>以方式4构建数据集，并用2中训练的模型在测试集上进行测试。

*进度与问题

## 6.27（painting）
<br>数据集（0-15个tracking作为训练集，16到20个tracking作为测试集）
<br>重新开始训练，训练了106个epoch,初始学习率0.01,优化器adam
<br>训练学习率
<br>![6.27_learning_rate](/image/6.27_learning_rate.png)
<br>训练损失
<br>![6.27_train_loss](/image/6.27_train_loss.png)
<br>验证精度
<br>![6.27_val_acc](/image/6.27_val_acc.png)
## 6.28 （painting）
<br>数据集（0-15个tracking作为训练集，16到20个tracking作为测试集）
<br>从上次最好的权重文件开始训练，训练了200个epoch,初始学习率0.01,优化器adam
<br>训练学习率
<br>![learning_rate](/image/6.28_learning_rate.png)
<br>训练损失
<br>![train_loss](/image/6.28_train_loss.png)
<br>验证精度
<br>![val_acc](/image/6.28_val_acc.png)
## 6.29 （painting）
<br>数据集（全部做训练并且测试精度）
<br>从新开始训练，训练了100个epoch,初始学习率0.01,优化器adam
<br>训练学习率
<br>![learning_rate](/image/6.29_learning_rate.png)
<br>训练损失
<br>![train_loss](/image/6.29_train_loss.png)
<br>验证精度
<br>![val_acc](/image/6.29_val_acc.jpeg)
## 7.2 （painting，数据集去除了地面）
<br>数据集（0-18个tracking作为训练集，19到20个tracking作为测试集）
<br>从新开始训练，训练了150个epoch,初始学习率0.01,优化器adam
<br>训练损失
<br>![train_loss](/image/7.2_train_loss.png)
<br>验证精度
<br>![val_acc](/image/7.2_val_acc.png)
<br>验证损失
<br>![val_loss](/image/7.2_val_loss.png)
## 7.3 （painting，数据集未过滤）
<br>数据集（每一个场景的前4/5作为训练集，后面1/5作为测试集）
<br>从新开始训练，训练了150个epoch,初始学习率0.01,优化器adam
<br>训练损失
<br>![train_loss](/image/7.3_train_loss.png)
<br>验证精度
<br>![val_acc](/image/7.3_val_acc.png)
## 7.3 （纯点云，数据集未过滤）
<br>数据集（每一个场景的前4/5作为训练集，后面1/5作为测试集）
<br>从新开始训练，训练了150个epoch,初始学习率0.01,优化器adam
<br>训练损失
<br>![train_loss](/image/7.5_train_loss.png)
<br>验证精度
<br>![val_acc](/image/7.5_val_acc.png)

## 8.23 (对预测网络采用大模型得到的数据作为真值进行训练)
<br>数据集(大模型对kitti进行语义分割得到的8008个数据)
<br>导入原始权重文件数据进行训练，训练了15个epoch,初始学习率为0.001，优化器adam
<br>以下为不进行预测网络的精度测试情况(间隔1fp,间隔2fp)
<br>![interval_1fp](/image/间隔1fp精度.png)
<br>![interval_2fp](/image/间隔2fp精度.png)
<br>以下为经过预测网络的精度测试情况(预测1fp,预测2fp)(采用原始权重文件)
<br>![pred_1fp](/image/预测1fp.png)
<br>![pred_2fp](/image/预测2fp.png)
<br>之后我采用经过kitti数据进行重新训练的网络进行精度测试(1fp为0.55，2fp为0.46)均比不过
