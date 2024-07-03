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
* 完成动态带宽条件变化的脚本，测试数据传输时延、检测帧率等参数，确保带宽条件被改变。
* 待完成

* 进度与问题

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
