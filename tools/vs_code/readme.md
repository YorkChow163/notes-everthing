# VS CODE

## 安装文件
去[官网](https://code.visualstudio.com/)下载最新文件

## 安装插件

* 管理员打开VS CODE

* 搜索安装以下插件
    * markdown checkbox
    * Paste Image
    * markdown preview enhanced
    * markdown table prettifier  
    ![](assets/2017-11-19-17-54-51.png)  
    
* 设置past image
    * ctrl+shift+p
    * 输入设置-》打开工作区设置
    * 选择pase image configration,做如下修改；
    
    ```
    {
    "pasteImage.path": "${currentFileDir}/assets/",
    "pasteImage.basePath": "${currentFileDir}/assets/",
    "pasteImage.prefix": "./assets/"
    }

    ```

## 提交git

* 查看提交文件，添加注释，点击√提交   
    ![](assets/2017-09-28-11-00-00.png)
* 点击三个点，点``推送``文件到服务器  
    ![](assets/2017-09-28-11-01-02.png)

## 常用快捷键

| 快捷键        | 作用             |
|---------------|------------------|
| ctrl+,        | 打开用户设置     |
| ctrl+`        | 打开控制台命令   |
| ctrl+p        | 查找文件         |
| ctrl+shift+p  | 执行命令         |
| ctrl+shift+m  | 预览markdown文件 |
| ctrl+k,ctrl+s | 设置快捷键       |
