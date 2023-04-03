# Java-TestNG-BrowserMobProxy

Configure ``LT_USERNAME`` and ``LT_ACCESS_KEY`` in ``src\main\java\SampleBM.java``


Problem statement: To capture the network logs at different instances of the automation tests.

Use case: Suppose you want to run an web automation script where you are performing n number of actions. After each set of actions, you would like to capture the network logs of the actions that has been executed.

Solution: Using a proxy called BrowserMob Proxy, we can capture the network calls after each commands.

What is a Browser Mob Proxy?

BrowserMob Proxy is a tool used for capturing and manipulating network traffic in web applications. It allows you to monitor and intercept HTTP/HTTPS traffic between a web browser and a web server. BrowserMob Proxy can be used for a variety of tasks such as performance testing, security testing, and web scraping.

With BrowserMob Proxy, you can capture HTTP traffic and analyze it for various purposes. It also provides the ability to manipulate requests and responses in real-time. For example, you can modify headers, add or remove cookies, and even modify the HTML of the page being loaded.

BrowserMob Proxy can be used with various programming languages such as Java, Python, and Ruby. It also has a REST API that allows you to programmatically control the proxy and retrieve data.

Overall, BrowserMob Proxy is a useful tool for developers, testers, and security professionals who need to monitor and manipulate network traffic in web applications.

For more details, please visit : https://github.com/lightbody/browsermob-proxy and https://bmp.lightbody.net/

How to capture the network logs using BrowserMob Proxy?

Step 1: Download the browsermob proxt from below URL: https://bmp.lightbody.net/
Step 2: Paste it in the same folder where your code is present.
Step 3: Add the browsermob-core dependency to your pom:

    <dependency>
        <groupId>net.lightbody.bmp</groupId>
        <artifactId>browsermob-core</artifactId>
        <version>2.1.5</version>
        <scope>test</scope>
    </dependency>

Step 4:  Start a proxy using net.lightbody.bmp.BrowserMobProxy:

     BrowserMobProxy proxy = new BrowserMobProxyServer();
    proxy.start(0);
    // get the JVM-assigned port and get to work!
    int port = proxy.getPort();
    //...

Step 5: Download Tunnel from below link and save it in the same folder where you saved the Browsermob proxy.
  https://www.lambdatest.com/support/docs/local-testing-macos/

Step 6: Start the tunnel. Use below code for reference.

        t = new Tunnel();
        HashMap<String, String> options = new HashMap<String, String>();
        options.put("user", "your_LT_username");
        options.put("key", "your_LT_accesskey");
        options.put("proxyHost",hostIp);
        options.put("proxyPort", portn);
        options.put("ingress-only", "--ingress-only");          //mandatory while using BM proxy
        options.put("tunnelName",portn);
        //start tunnel
        t.start(options);

Step 7: Set the desired capabilities:

       ChromeOptions option = new ChromeOptions();

        option.addArguments("--ignore-certificate-errors");
        option.setProxy(seleniumProxy);
        option.setAcceptInsecureCerts(true);
        option.addArguments("--disable-backgrounding-occluded-windows");

        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(ChromeOptions.CAPABILITY, option);
        capabilities.setCapability("browserName", browser);
        capabilities.setCapability("version", version);
        capabilities.setCapability("platform", platform);
        capabilities.setCapability("tunnel",true);
        capabilities.setCapability("tunnelName",portn);

Step 8: Set the remote connection:

        driver = new RemoteWebDriver(new URL("https://username:accesskey@hub.lambdatest.com/wd/hub"),capabilities);


Step 9: Get the HAR data.

   proxy.newHar("HomePage");

        driver.get("https://www.google.com/");

        driver.findElement(By.name("q")).sendKeys("LambdaTest");
        driver.findElement(By.name("q")).sendKeys(Keys.ENTER);
        Thread.sleep(5000);

        driver.get("https://lambdatest.com");

        // get the HAR data
        Thread.sleep(5000);
        Har har = proxy.getHar();

Step 10: Capture the HAR logs in the path same as where Browsermob proxy and tunnel are located.

        try {
            har.writeTo(new File("C://Users//deepanshu//Downloads//Java-TestNG-BrowserMobProxy-master 2//Java-TestNG-BrowserMobProxy-master"+proxy.getPort()+"homepage.har"));

        } catch (IOException e1) {
            e1.printStackTrace();
        }


That All!

You will get the logs in the desired location you mentioned.
Please refer to the code snippet for better understanding.




 
