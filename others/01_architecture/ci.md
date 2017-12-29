

#### 持续集成总结

 主流程：

 1 编译， 单元测试 （小规模）
 2 集成测试 （中规模）
 3 质量检查
 
 4 打包& 发布（distribute）-> publish




 
 
 5 UAT （功能测试）-> Qqulity Test（人工测试） - > Production


   Provision as code ( 1 kvm , 2 software )

  Targeting a deployment environment
  
 1 环境准备（Provision） -> deployment ( copy -> stop -> start ....) ->test

 人工测试：
 1 环境准备（Provision）? -> smoktest


6 product like enviroment

部署

 1 Push new artifact to server.
2 Stop the web container process.
3 Delete the old artifact and its extracted files.
4 Deploy the new artifact.
5 Start the web container process. 