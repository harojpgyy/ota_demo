# 物联网平台5.0  Android SDK 详细文档 版本3.0
  libs:fotaLibs3_0.0.1

# 集成介绍
  1.AndroidManifest.xmd 中需要集成在平台上创建的项目ID
       <meta-data
           android:name="fota_configuration_product_id"
           android:value="fota/xxxxxxxx"
           tools:replace="android:value" />
       <meta-data
           android:name="fota_configuration_product_secret"
           android:value="fota/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
           tools:replace="android:value" />
           
  2.设备集成VIN 为当前设备的唯一编码,不能重复
  
  3.为满足动态挂载零件,所有的零件信息做成动态加入方式(必传)
  ------setUpgradePartsInfo(DeviceInfo deviceInfo)
  ------setUpgradePartsInfo(List<DeviceInfo> deviceInfo)
  
  4.零件信息参数--示例Android零件
      DeviceInfo deviceInfo = new DeviceInfo();
      deviceInfo.partNumber = "android_8888";         零件编码,代表此零件的唯一编码,建议以读取系统属性方式读取
      deviceInfo.partName = "Android";                零件名称,建议以读取系统属性方式读取
      deviceInfo.partHsn = "";
      deviceInfo.partSVer = "";                       零件版本号  
      deviceInfo.installAuto = OtaConstant.SerialPort;零件升级方式          
      deviceInfo.isMainProduct = false;               零件是否为主节点            
      deviceInfo.productDate = "2020-10-1";
      deviceInfo.customInstallCallback = new InstallBean();  当前零件的升级实现
      Trace.d(TAG, "parts:" + deviceInfo.toString());


# 功能介绍
  #类介绍
  SDK初始化
  ----OtaAgentPolicy.class
       #方法调用
        OtaAgentPolicy.getInstance()
                      .setContext(this)
                      .setVin(vin)              -设置vin码
//                    .setProjectInfo()         -设置Project信息 (可传参)不传参默认读取清单文件
                      .setDownloadFilePath(getFilesDir() + File.separator)      -设置下载地址
                      .commit();                -提交


  ----OtaApi.class
        #方法调用
            //传入当前设备的零件信息 有多少个传入多少个(必传)
            ------setUpgradePartsInfo(DeviceInfo deviceInfo)
            ------setUpgradePartsInfo(List<DeviceInfo> deviceInfo)

        #异步方法调用  -Asy
            ------registerAsy()         -设备注册接口调用
            ------configAsy()           -设备获取零件信息
            ------partsInfoUpAsy()      -设备上报零件信息
            ------checkAsy()            -设备检测升级信息
            ------taskEndedAsy()        -设备查询任务终止
            ------downloadAsy()         -设备开启任务下载
            ------installAsy()          -设备开启任务升级

        #同步方法调用  -Syn
            ------registerSyn()         -设备注册接口调用
            ------configSyn()           -设备获取零件信息
            ------partsInfoUpSyn()      -设备上报零件信息
            ------checkSyn()            -设备检测升级信息
            ------taskEndedSyn()        -设备查询任务终止
            ------downloadSyn()         -设备开启任务下载
            ------installSyn()          -设备开启任务升级

  #回调接口
   ------onRegistrarCallback.class
   ------onConfigCallBack.class
   ------onCheckCallBack.class
   ------onInstallCallBack.class
   ------onDownloadCallBack.class
   ------onTaskEndedCallBack.class

  #统一错误码
   ------Code.class

  #缓存数据类
   ------DataManager.class、

  #数据库管理类
   ------ParamsDBManager.class
   ------ParamsDBHelper.class

  #常用常量属性管理类
   ------OtaConstant.class