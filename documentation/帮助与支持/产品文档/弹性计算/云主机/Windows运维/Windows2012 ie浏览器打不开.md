**Windows2012 ie浏览器打不开**

**问题现象：**

打开ie浏览器，提示如下错误：

![1.png](https://img1.jcloudcs.com/cms/8d7388b9-61f2-43c2-bdf7-4acb0afc4d8b20170913163138.png)

**解决办法：**

1.右键开始，选择运行。在弹出的窗口中输入"secpol.msc"命令。

![2.png](https://img1.jcloudcs.com/cms/9c63e3c8-32ab-4c62-b9aa-cd15bb94dca220170913165207.png)

![3.png](https://img1.jcloudcs.com/cms/6b26b01c-8da1-4093-bca8-947a84eda64120170913165321.png)

2.在弹出的页面中，点击选择“安全设置-本地策略-安全选项”菜单，在依次展开的页面中进行对应的设置。在打开的安全选项右侧页面中，点击找到“用户账户控制：用于内置管理员账户的管理员批准模式”选项。

![4.png](https://img1.jcloudcs.com/cms/789204e0-b47a-4e72-b061-710bb600150420170913165312.png)

3.双击打开页面设置之后，我们点击将该选项设置为"已启用"按钮，然后点击确定按钮保存对账户安全策略的设置。

![5.png](https://img1.jcloudcs.com/cms/1423f7f3-5027-492d-8310-a2e374ac51fe20170913165357.png)

4.重启服务器生效。

如无法解决您的问题，请向我们提工单。