# 目录
<!-- MarkdownTOC autolink="true" -->

- [ui测试](#ui%E6%B5%8B%E8%AF%95)
	- [webui 测试（selenium）](#webui-%E6%B5%8B%E8%AF%95%EF%BC%88selenium%EF%BC%89)
		- [下载](#%E4%B8%8B%E8%BD%BD)
		- [项目文件](#%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6)
		- [启动项目](#%E5%90%AF%E5%8A%A8%E9%A1%B9%E7%9B%AE)
		- [常用api](#%E5%B8%B8%E7%94%A8api)
	- [appui 测试（appium）](#appui-%E6%B5%8B%E8%AF%95%EF%BC%88appium%EF%BC%89)
		- [下载](#%E4%B8%8B%E8%BD%BD-1)
			- [mac 下 android环境搭建](#mac-%E4%B8%8B-android%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)
				- [ios环境搭建](#ios%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)
		- [文件](#%E6%96%87%E4%BB%B6)
		- [启动项目](#%E5%90%AF%E5%8A%A8%E9%A1%B9%E7%9B%AE-1)
		- [常用api](#%E5%B8%B8%E7%94%A8api-1)
- [jmeter测试](#jmeter%E6%B5%8B%E8%AF%95)

<!-- /MarkdownTOC -->


# ui测试
## webui 测试（selenium）
#### 下载
[webDriver](http://chromedriver.storage.googleapis.com/index.html) 
#### 项目文件
pom.xml
```xml
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>3.8.1</version>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-server</artifactId>
            <version>3.8.1</version>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-api</artifactId>
            <version>3.8.1</version>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-remote-driver</artifactId>
            <version>3.8.1</version>
        </dependency>
```
testing_uat.xml。 

可通过`@Parameters({ "properties", "browsertype", "browserversion", "remoteIP" })`应用到相应的方法。    
`@Test(groups = { "setContactField"})` 标记所属组。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="自动化测试" parallel="tests"  thread-count="4" verbose="1" >
    <parameter name="properties" value="src/main/resources/configUAT.properties" />
    <parameter name="browsertype" value="chrome" />
    <parameter name="browserversion" value="" />
    <parameter name="remoteIP" value="" />
    <test name="TestContact01">
        <groups>
            <run>
                <include name="baseTest"  />
                <include name="setContactField"  />
            </run>
        </groups>
        <classes>
            <class name="com.test.Test01"/>
        </classes>
    </test>
 </suite>
```
#### 启动项目
修改配置文件 configUAT.properties；
run testing_uat.xml 即可；

#### 常用api
```java
WebDriver driver = new FirefoxDriver() 	//获取相应浏览器的driver
WebElement webElement = driver.findElement ( By.name ( "userName" ) ); //获取html元素
webElement.sendKeys ("sendKeys"); webElement.click(); //webElement    常用js动作

JavascriptExecutor jse = ((JavascriptExecutor) driver);
jse.executeScript("window.scrollBy(50,200)"); //直接执行js，也有异步方法
```
## appui 测试（appium）
#### 下载
[appium](https://bitbucket.org/appium/appium.app/downloads/)  
[androidSDK](https://developer.android.com/studio/#downloads)  
若选择命令行安装，还需手动安装sdk
##### mac 下 android环境搭建 
1. 安装 sdk
```bash
brew cask install android-sdk 
sdkmanager "build-tools;28.0.1"  "platform-tools"  "system-images;android-28;google_apis;x86_64" "emulator"
```
创建Android模拟器 `avdmanager create avd -n test -k 'system-images;android-28;google_apis;x86_64'`  
启动android模拟器 `emulator -avd test`  
2. 获取设备号，包名，主页。
```bash
adb devices
adb shell "dumpsys window | grep mCurrentFocus"
```
3. 查找元素 
```bash
uiautomatorviewer
```
###### ios环境搭建

#### 文件
pom.xml
```pom
        <dependency>
            <groupId>io.appium</groupId>
            <artifactId>java-client</artifactId>
            <version>4.1.2</version>
        </dependency>
```

testing_uat.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="autotest">
	<parameter name="Platform_Name" value="Android" />
	<parameter name="Platform_Version" value="5.1" />
	<parameter name="Device_Name" value="R8W4D6PB99999999" />
	<parameter name="udid" value="R8W4D6PB99999999" />
	<parameter name="App_Package" value="com.zjgsh.dev" />
	<parameter name="App_Activity" value="com.hand.hrms.activity.SplashActivity" />
	<parameter name="Driver_Url" value="http://127.0.0.1:∏4723/wd/hub" />
	<parameter name="properties" value="src/main/resources/configHmapAppTest.properties" />
	<test name="HmapAppRegistTest1" preserve-order="true" verbose="10">
		<groups>
			<run>
				<include name="BaseTest" />
				<include name="HmapRegist_Test1" />
			</run>
		</groups>
		<classes>
			<class name="AppIM.Cases.HmapRegistTest1" />
		</classes>
	</test>
 </suite>
```
#### 启动项目
1. 启动appium
2. 修改配置文件
```
Device_Name=emulator-5554
App_Package=com.zjgsh.dev
App_Activity=com.hand.hrms.activity.SplashActivity
```
3. run testing_uat.xml 

#### 常用api
```java
AndroidDriver driver = new AndroidDriver(new URL(Driver_Url), capabilities); 
WebElement clickMe = driver.findElementByXPath("//*[@id='uniqlo-footer']/footer/div[5]/a");
clickMe.click();
```
# jmeter测试
略


