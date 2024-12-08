# zllc_2025_homework
嵌入式新生在此提交作业和学习日志
1.	系统架构
系统的层级和各层功能，列举所使用的第三方中间件和模块，如RTOS、文件系统、日志系统、GUI、协议框架等。宿主机开发环境和调试环境。

协议架构：
通信协议UASRT

宿主机开发环境：
		Cube MX+keil5+vscode+EIDE

调试环境：
硬件调试：J-Link SW接口
软件调试：
			Keil 5 debug
通过keil 5 debug里的switch窗口监测速度解算数据、pwm占空比数据、中断回调函数的调用情况以及代码逻辑问题
			Ozone V3
通过ozone实时监测电机编码器数据，可视化电机转速，用于增量式PID调参
Vofa+,波特律动 串口调试助手
监测串口通信发送的数据是否正确
2.	运行流程
软件整体的运行流程、数据流向和处理过程。
Esp32接收移动端数据，通过串口通信传输给单片机，
单片机先通过LIFT数据判断机器人模式
0：遥控模式
通过对摇杆数据进行映射，x轴坐标映射为角速度，y坐标轴映射为线速度，然后将其代入差速公式中解算四轮的目标速度，再作为目标值与编码器测出的实际值代入PID计算函数中，实现PID闭环控制
通过解算剩余5个按键数据实现对应功能
			FORWARD(1):向前抬腿
			BACK(1)：向后抬腿
			RIGHT(1):向右转
			LEFT(1)：向左转
			JUMP(1):全速执行
1：自动模式
		上下位机通过串口通信传输车身速度和转向角速度，再代入底盘解算函数中计算四轮的目标速度，再作为目标值与编码器测出的实际值代入PID，实现自动巡线和自动避障

3.	重点功能
核心功能点描述，解决了何种问题，使用了何种技术。
1.	遥控功能：通过对摇杆数据进行映射，x轴坐标映射为角速度，y坐标轴映射为线速度，然后将其代入差速公式v_lf=v_lr=V-wR,v_rf==v_rr=V_wL中解算四轮的目标速度，再作为目标值与编码器测出的实际值代入PID计算函数中，实现遥控功能
问题：接收数据不稳定
技术：串口通信的DMA模式
2.	自主巡线
	上下位机通过串口通信传输车身速度和转向角速度，再代入底盘解算函数中计算四轮的目标速度，再作为目标值与编码器测出的实际值代入PID，实现自动巡线和自动避障
		3.PID闭环控制
         问题：初始版本小车响应慢
		技术：通过ozone可视化界面调节Kp,Ki,Kd参数，实现精准的闭环控制
4.	软件测试
测试方案简介、测试执行情况和结果。
1.	编码器计数测试
测试电机编码器是否正常工作
结果：正常
2.	台阶测试
测试舵机上台阶的可行性 
结果：初始版本成功概率较低，经过调整重心后成功概率显著提高