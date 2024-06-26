# multi-model-object-detecion
mobicom deadline schedule

## 朱永
* 分别写出本地读ROI、预测ROI、真实label3种情况下的可视化代码，用于比较云端回传ROI的影响。
* 解决ROI云端回传八叉树下采样问题。（何伟博辅助部署）
* 下采样反馈机制，调节数据传输体量。（何伟博辅助部署）

## 何伟博
* 完成动态带宽条件变化的脚本，测试数据传输时延、检测帧率等参数，确保带宽条件被改变。

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
