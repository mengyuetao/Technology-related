
## 持续集成总结
新增容器化的流程k8s
主流程：
 1 编译， 单元测试 （小规模）
 2 集成测试 （中规模）
 3 质量检查
 4 打包& 发布（distribute）-> publish
 5 UAT （功能测试）-> Qqulity Test（人工测试） - > Production

Provision as code ( 1 kvm , 2 software )
Targeting a deployment environment

## Provision 环境准备

- Old: Provision（puppet）-> deployment ( copy -> stop -> start ....) ->test
- New: Provision（docker build push with version）-> deployment ( pull from rep -> k8s redeply ....) ->test


## product like enviroment 部署
1 Push new artifact to server（pull from rep using k8s）.
2 Stop the web container process (start using kubectl ).
3 Delete the old artifact and its extracted files (not necessary).
4 Deploy the new artifact.
5 Start the web container process.
