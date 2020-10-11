/*!
 * MindPlus
 * mpython
 *
 */
#include <MPython.h>
#include <DFRobot_Iot.h>
// 函数声明
void obloqMqttEventT1(String& message);
// 静态常量
const String topics[5] = {"2018764108/苏国昊","2018764140/吴超华","","",""};
const MsgHandleCb msgHandles[5] = {NULL,obloqMqttEventT1,NULL,NULL,NULL};
// 创建对象
DFRobot_Iot myIot;


// 主程序开始
void setup() {
	mPython.begin();
	myIot.setMqttCallback(msgHandles);
	myIot.wifiConnect("hhh", "5832615ASD");
	while (!myIot.wifiStatus()) {yield();}
	display.setCursorLine(1);
	display.printLine("wifi连接成功了");
	myIot.init("192.168.43.36","602","","iot", topics, 1883);
	myIot.connect();
	while (!myIot.connected()) {yield();}
	display.setCursorLine(2);
	display.printLine("MQTT连接成功");
}
void loop() {
	if ((buttonA.isPressed())) {
		myIot.publish(topic_0, "2018764140吴超华");
		display.setCursorLine(3);
		display.printLine("发送成功");
		buzz.play(DADADADUM, Once);
	}
}


// 事件回调函数
void obloqMqttEventT1(String& message) {
	buzz.play(DADADADUM, Once);
	display.setCursorLine(4);
	display.printLine(message);
}
