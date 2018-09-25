# ui测试
## webui 
### selenium常用api
```java
WebDriver driver = new FirefoxDriver() //根据配置获取相应浏览器的driver
WebElement webElement = driver.findElement ( By.name ( "userName" ) ); // By   html元素
webElement.sendKeys ("sendKeys"); webElement.click();  //webElement    常用js动作

JavascriptExecutor jse = ((JavascriptExecutor) driver);
jse.executeScript("window.scrollBy(50,200)"); //直接执行js，也有异步方法
```
## appui 
### 常用api
```java

```

