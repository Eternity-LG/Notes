### Jmeter之java请求案例

#### 1.java代码

主要包含四个方法对应Jmeter调用jar包时对应的方法:	
分别是:

	public class Test2 extends AbstractJavaSamplerClient {
		public static void main(String[] args) {
		Test2 test = new Test2();
		Arguments arg0 = test.getDefaultParameters();
		JavaSamplerContext argResult = new JavaSamplerContext(arg0);
		test.setupTest(argResult);
		test.runTest(argResult);
	}
	public Arguments getDefaultParameters() {
		Arguments args = new Arguments();
	//		 添加默认参数及对应值 //参数名称：id 参数值：123456789
		args.addArgument("path", "http://192.9.180.42/abside/modulea-aase/aase/index");
		args.addArgument("Content-Type", "application/json");
		args.addArgument("X-ABX-ModuleName", "ModuleA");
		args.addArgument("X-ABX-TradeName", "trade%2F%E5%AE%A2%E6%88%B7%E4%BF%A1%E6%81%AF%E7%BB%B4%E6%8A%A4");
		args.addArgument("X-ABX-TradeTitle", "%E5%AE%A2%E6%88%B7%E4%BF%A1%E6%81%AF%E7%BB%B4%E6%8A%A4");
	//		args.addArgument("X-ABX-OID",tempString);
		args.addArgument("X-ABX-StepKey", "");
		args.addArgument("X-ABX-SessionID", "");
		return args;
	}
	private String OID;
	private String tradeName;
	private String tradeTitle;
	private String sessionID;
	private String stepKey;
	private String path;
	@Override
	public void setupTest(JavaSamplerContext context) {
		String str = UUID.randomUUID().toString();
		String tempString = str.substring(0, 8) + str.substring(9, 13) + str.substring(14, 18) + str.substring(19, 23) + str.substring(24);
		OID = tempString;
		path = context.getParameter("path");
		tradeName = context.getParameter("X-ABX-TradeName");
		tradeTitle = context.getParameter("X-ABX-TradeTitle");
		sessionID = context.getParameter("X-ABX-StepKey");
		stepKey = context.getParameter("X-ABX-StepKey");
		System.out.println(OID);
	}
	/**
	 * 每个线程执行多次,相当于loadrunner的Action()方法 (non-Javadoc)
	 */
	public SampleResult runTest(JavaSamplerContext arg0) {
		boolean if_success = true;// 测试结果标记位
		SampleResult sr = new SampleResult(); // 为避免多线程问题，设置sr为局部变量
		sr.setSampleLabel("java请求");
		CountDownLatch countDownLatch = new CountDownLatch(4);
		try {
			// 准备第一次发送请求的map
			sr.sampleStart();
			Map<String, Object> map = new HashMap<String, Object>();
			map.put("OID", OID);
			map.put("tradeName", tradeName);
			map.put("tradeTitle", tradeTitle);
			map.put("StepKey", stepKey);
			map.put("path", path);
			map.put("sessionID", sessionID);
			// websocket建立链接
			WebSockets webSocket = new WebSockets("ws://192.9.180.42/abside/message/message/websocket", countDownLatch, map);
			webSocket.createWebSocket();
			Map<String, Object> initMessage = new HashMap<>();
			Map<String, String> data = new HashMap<>();
			initMessage.put("type", "init");
			initMessage.put("id", OID);//原值不进行加密
			initMessage.put("data", data);
			webSocket.register(initMessage);// sr.setResponseData("data return by server", ""); //第一个参数 设置JMeter GUI
		// "查看结果树" 请求对应的"响应数据" // 执行压测前 建议注释掉
			System.out.println("websocket注册成功");
	//			new HttpURLConnectionDemos().doPost(map);
	//			System.out.println("---------------------- 请求结束 ----------------------");
			sr.setResponseData("响应结果", "utf-8"); // 第二个参数 为编码， 设置JMeter GUI "取样器结果" DataEncoding: utf-8
			sr.setDataType(SampleResult.TEXT); // 设置JMeter GUI "取样器结果" Data type ("text"|"bin"|""):text
			sr.setResponseMessageOK(); // 设置JMeter GUI "取样器结果" Response message: OK
			sr.setResponseCodeOK(); // 设置JMeter GUI "取样器结果" Response code: 200
			if_success = true;
		} catch (Exception e) {
			if_success = false; // 请求失败
			sr.setResponseCode("500"); // 设置JMeter GUI "取样器结果" Response code: 500
			e.printStackTrace();
		} finally {
			try {
				countDownLatch.await();
				sr.sampleEnd();
				sr.setSuccessful(if_success);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		return sr;
	}
	public void teardownTest() {
	}
	}

#### 2.jar包打包

#####  第一步选择可运行jar包
![image-20201105140907575](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20201105140907575.png)
##### 第二步选择主类对应主函数(没有的话就创建一个)
![image-20201105141034161](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20201105141034161.png)

#### 3.Jmeter配置

 如图所示:
 选择需要导入的jar包
![image-20201105140726638](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20201105140726638.png)

