[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-f059dc9a6f8d3a56e377f745f24479a46679e63a5d9fe6f495e02850cd0d8118.svg)](https://classroom.github.com/online_ide?assignment_repo_id=6196725&assignment_repo_type=AssignmentRepo)
# iw04
iOS assignment 4: Image Classification App.

作业 4-1 
  请基于模板工程(starter/HealthySnacks)，运用CoreML开发一个利用卷积神经网络分类snacks的App

功能要求如下：

1. 通过CreateML基于给定snacks数据集训练图像分类模型
2. 可针对拍照和相册中选择的图片利用训练好的模型进行snacks分类
3. 在屏幕上展示神经网络模型的分类结果

非功能需求如下：

1. 面对不属于数据集内给定snacks类别以外的图片，或模型本身把握不准的图片，给出“我不确定”的判断


作业 4-2
  请基于模板工程(starter/HealthySnacks)，运用CoreML开发一个利用卷积神经网络分类健康和非健康snacks的App

功能要求如下：

1. 改造snacks数据集，形成可供训练分类健康snacks模型的数据集
1. 通过CreateML基于改造的数据集训练图像分类模型
2. 可针对拍照和相册中选择的图片利用训练好的模型进行健康/非健康snacks的分类
3. 在屏幕上展示神经网络模型的分类结果

非功能需求如下：

1. 面对不属于数据集内给定snacks类别以外的图片，或模型本身把握不准的图片，给出“我不确定”的判断

作业 4-3 （无需提交结果，自学）

通过train_model.ipynb，学习PyTorch训练模型流程，以及如何将PyTorch模型转换为mlmodel的格式

## 实验过程

在完成模型训练后，查看模型的输入，CVPixelBuffer

在网络上找到了一个UIimage 到 CVPixelBuffer 的函数

    func imageToCVPixelBuffer(image:UIImage) -> CVPixelBuffer? {

      ...

    }

在processObservations中根据confidence 完成准确性的判断

    func processObservations(for request: VNRequest, error: Error?) {
            
      ...      
      
      let identifier = results[0].identifier
      let confidence = results[0].confidence
      if confidence < 0.6 {
        self.resultsLabel.text = "I'm Not Sure\n"
      } else {
        self.resultsLabel.text = String(format:"%@: %.1f%%\n", identifier, confidence * 100)
      }
                
      ...

    }

通过多线程完成加载

    DispatchQueue.main.async {
      let handler = VNImageRequestHandler(cvPixelBuffer: pixelBuffer)
      do {
          try handler.perform([self.classificationRequest])
          //种类识别
      } catch {
          print("Failed to perform classification: \(error)")
      }
    
      do {
          try handler.perform([self.healthyclassificationRequest])
          //健康识别
      } catch {
          print("Failed to perform classification: \(error)")
      }
                
    }

由于硬件原因，我未能完成拍照识别的功能，我在群里已经加以说明