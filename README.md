# Requirements
* Docker
* Selenoid or Selenium Hub or Appium etc. launched on some host

# Instruction how to get GGR working

* Navigate to folder where you want to ggr folders live in
* Create http auth credentials for future using in url to connect tests with ggr
  * use command `htpasswd -bc $PWD/users.htpasswd <USERNAME> <PASSWORD>`
  * you should remember or save somewhere that `<USERNAME>` and `<PASSWORD>` valuesm cause AQA team needs them to connect to ggr
* Create quota config file
  * create directory and navigate into it using command `mkdir quota && cd quota`
  * create config file using command `touch <USERNAME>.xml`
  * use any text editor to fill that file with content like following:  

```xml
  <qa:browsers xmlns:qa="urn:config.gridrouter.qatools.ru">
<browser name="chrome" defaultVersion="62.0">
    <version number="62.0">
        <region name="1">
            <host name="localhost" port="4445" count="1"/>
        </region>
    </version>
</browser>
<browser name="firefox" defaultVersion="57.0">
    <version number="57.0">
        <region name="1">
            <host name="localhost" port="4445" count="1"/>
        </region>
    </version>
</browser>
</qa:browsers>
```

  * now you should change actually the paths to Selenoids, Selenium Hubs, etc..
    * `name` - IP or hostname to machine
    * `port` - port on which Selenoid are listening
    * `count` - that param is needed for evenly distributing sessions between environments, if you have 4 selenoids with `-limit 10` you can write here any value, and ggr will spread sessions evenly between all of them, and if you have 2 selenoids with `-limit 2` and `-limit 50` you need to set `count` to 2 and 50
    * you should repeat these steps to show all possible combinations (that configuraion above is for situation when ggr and selenoid launched on same host, and selenoid can give only chrome-62 and ff-57 browsers)  
* Launch ggr using command:  
  ```docker run -d --name ggr -v $PWD:/etc/grid-router:ro --net host aerokube/ggr:latest-release```  
* Check that ggr is running and were no errors during launch using command:  
  ```docker logs ggr```  
* Expected output should be look like:
```
alexanderpopov@Alexander-MBP docker logs ggr
2017/12/14 10:03:41 Users file is [/etc/grid-router/users.htpasswd]
2017/12/14 10:03:41 Loading configuration files from [/etc/grid-router/quota]
```
* Notify AQA team and provide them `<USERNAME>` and `<PASSWORD>` for ggr