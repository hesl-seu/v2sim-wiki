# Download CS
**Note: Due to the restriction of the service provider, this section may only fit for users in China Mainland. Some service is only provided in simplified Chinese.**

## Disclaimer

If you use `Download CS` in V2Sim, it is deemed that you agree to the following statement:
+ V2Sim will not collect your key, and your key is stored locally. 
+ You must agree the EULA provided by AMap to use the service. 
+ V2Sim, together with its developers, is not responsible for the abuse of the applied key in any situation.

## Introduction
V2Sim is able to download EVCSs automatically from AMap. However, you have to prepare a key on your self. Currently, applying for a key is free in AMap. Visit the [official site](https://lbs.amap.com/) to do this.

## Apply for a key
Create an account if you do not have one. Login to the console. In the left sidebar, click "应用管理"——"我的应用" to view your applications.

Click the blue button "创建新应用" on the top right. Input "应用名称" and "应用类型", and then click "新建". In the newly created application, click "添加Key" on the top right. Input "Key名称" and select "Web服务" for "服务平台".

Then you will get a key consisting of digits and letters. Put this key into page "Download CS" in V2Sim main GUI.

## Download EVCS in V2Sim
Just open your case, fill in your key in "Download CS" page, and click "Download". The process is automatic, watch the command line prompt to view the progress. Notice that you must have enough quota for searching POI. A free key may have a very low quota so that you may not able to download all of the EVCS in the selected area.

**Note:**
1. Error may happen during the download procedure. In this case, please copy the file `buf.csv` in V2Sim root folder to your case and rename it to `cs.csv`. It contains the cache of downloaded CS.

2. If you are using V2Sim-GUI, you may have to reopen the GUI to view the downloaded CS.