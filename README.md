# mobile-interprocess-communication
设备设置页面DeviceSettingActivity--跳转--授权管理页SmartHardwareActivity，是从物联进程跳转到主进程的。
intent.setClass(mAppBaseActivity.getOutActivity(), SmartHardwareActivity.class);
其中mAppBaseActivity代表的是DeviceSettingActivity，getOutActivity表示使用代理Activity进行进程间跳转。
授权管理页SmartHardwareActivity--跳转--删除or全部成员页DeviceDeleteFriendActivity，二者均在主进程中。
 Intent intent = new Intent(SmartHardwareActivity.this, DeviceDeleteFriendChooserActivity.class);
授权管理页SmartHardwareActivity--跳转--DevTransferAuthorizeActivity设备转让须知页，是从主进程跳转到物联进程中。
Intent intent=new Intent();
SmartDevicePluginLoader.getInstance().launchPluginActivityForResult(
this,app,app.getAccount(),intent,"com.tencent.device.activities.DevTransferAuthorizeActivity",0,null,SmartDevicePluginProxyActivity.class);

下面要把授权逻辑全部移到物联进程中，资源挪移至QQ_6.3.8\DevicePlugin\res\layout中。
AndroidManifest.xml中注册的授权Activity删除

SmartHardwareActivity中修改，删除、全部、转让改为主进程跳转至物联进程。注意请求码。

将删除、全部、转让成员页面放到移动至物联的包中--com.tencent.device.activities

将select_member_searchbar_background.xml移动至物联的layout中。添加select_member_searchbar_background.xml、skin_group_list_item_normal.9.png
和create_discussion_avatar_add.png和skin_searchbar_icon.png。

修改custom_commen_title.xml，添加右边按钮。添加top_button_right_selector.xml和skin_header_btn_disable.9.png和skin_header_btn_press.9.png和
skin_header_btn_normal.9.png

将authority_chooser_item.xml移动至物联的layout中。

测试中发现，逻辑移动到物联进程时，积累DeviceAuthorityChooser.java不能再继承BaseActivity了，由于该类是主进程的，所以需要继承QQSmartDevice
BaseActivity，继承这个Activity时，又遇到问题了。这个Activity本身就设置了titlebar和titlebar的点击事件，所以要把之前写得titlebar和点击事件
删除。很麻烦。

