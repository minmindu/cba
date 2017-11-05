# CBA Weather Data  

This project is to generate weather data for testing purpose. More interesting, it will also visualize the generated data into a map.
 
There are two parts of this project:
1. Generate weather data for certain cities for a country (Java App)
    <br>**A Scala version** is also available in https://github.com/minmindu/weather-data-scala
2. Visualise the weather data result set in a map for data analysis (JavaScript)


<br>
<br>

## PART 1 -  WEATHER DATA GENERATOR

### Introduction

The weather data generator is written in Java. 

It takes country name (case-insensitive )as the only one parameter as an input and will generate a txt file with the current weather data in a required format for certain cities within the define country.
It uses "AUSTRALIA" as a default country if no input is defined. 

### Detailed Design

**1. Weather Data API**

The application is using a Open Weather API (http://api.openweathermap.org/data/2.5/weather).
It is to get the real weather data that the API provided at the time when you run the program.

Note the API requires an API Key. A Free API Key has already been populated in the application for testing only. 
But this has limited call per day. Please utilise your own if you wish to use this API beyond testing. This can be done here: http://openweathermap.org/appid

**2. IATA Reference**

Since the business requires to show IATA (International Air Transport Association) code for cities, the application will search weather data for these cities, which have IATA code only, for a defined country.
It will take a reference data for IATA mapping (source data is in /src/main/resources/GlobalAirPortDatabase.txt) for testing only. 
Country name value is in the 5th column in the file (e.g. AUSTRALIA, USA, CHINA)


### How it work

The weather data generator is a maven project written in Java. Therefore, some basic dependency is required.

### Prerequisite
1. Java 8 is required to be installed before compile the project. 

You can run  the below command to check Java version in your environment. 
If Java version is NOT 1.8.x, then please go to http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html and install jdk1.8. 
```
java -version
```

2. Maven is required to be installed in your environment in order to compile and build the source code.
Run the below command to check if Maven is installed. 

If not, refer to <url>https://maven.apache.org/install.html for installation. 
<br>A simple sample to install Maven in Windows is also provided in <url>https://www.mkyong.com/maven/how-to-install-maven-in-windows/
```
mvn -version
```

### Build
1. Download the project to local laptop
2. Open the command line terminal
3. Go to weather folder in the project
4. Run the below Maven commands to build a jar file from source code. 
    * Clean
        ```
        mvn clean
        ```    
    * Compile
        ```
        mvn compile
    * Install
        ```
        mvn install
       ```      
       
### Run       
1. Run the below command in command line terminal after building the project 
(assuming that you are still in weather repository)

    ```  
    java -jar target/weather-data-1.0.jar CHINA
    ```  
    Or run the program by using the default country name AUSTRALIA
   ```  
   java -jar target/weather-data-1.0.jar
   ```  
    
2. Result file named "weatherdata.txt" will be generated in the current directory


### Note
The altitude figure in the testing data might be different from the provided sample data in the business requirement. 
It is because the calculation point is slightly differnt. 

For example, in the provided sample in the business requirement, altitude is 39 for SYD whereas in the generated data set from this application, altitude is 7 for SYD.
This application is simply calculating the airport's altitude rather than the whole city's average altitude.
Similar as longitude and latitude. 

<br>
<br>

## PART 2 - WEATHER DATA VISUALISATION

### Introduction

Apart from the Java application to generate weather data, this project also provides a sample example of how to use weather data for data analysis. 
The 2nd part of this project is to visualise the generated result set in Google Map. 

### Detailed Design

Weather data visualisation application is written in JavaScript. 
It reads data from the generated weatherdata.txt file, which is done in Part1, and render data in Google Map.

It also adds a real-time temperature layer, which is provided by Open Weather API
Temperature layer indicates rough temperature across the world. The cooler color indicates lower temperature whereas warmer color describes higher temerature.

JavaScript files are already developed and provided in this project source code (src/main/js). weatherMap.html is the web page for weather data visualisation. 
However, you need to set up a server to run it as it reads data from a provided flat file. 

Go to website <url>**http://weathermap.davidxian.com/** to experience how it works. The weather data in this website was captured at 2017-10-30T04:06:00Z for Australia.

If you would like to deploy it in local server, please follow the below steps to set up your environment. 

### Prerequisite
1. Web server is required to set up in local laptop in order to run weatherMap.html, which reads data from the generated result set.
    <br> The below instruction is to use XAMPP, which provides Apache HTTP Server 
2. file name "**weatherdata.txt**" is hard-coded in JavaScript weatherMap.html. Therefore, the exact file name is required for weatherMap.html to render data. 
 
### Build 
1. Download and install XAMPP application from https://www.apachefriends.org/download.html
2. Find XAMPP installation path after installation (e.g. /Applications/XAMPP)
2. Go into sub-folder '**htdocs**' under XAMPP installation repository (e.g. /Applications/XAMPP/htdocs)
3. Copy the below files into the htdocs folder:
    * .../weather/src/main/js/jquery-1.11.3.js
    * .../weather/src/main/js/wealtherMap.html
    * .../weather/weatherdata.txt (the result data file that is generated in Part 1. Path might be different depending on where you run the program)

### Run
1. Ensure XAMPP is running and Apache Web Server is running (it would be started by default after installation) 
 **NOTE** Please note that the API Key in the program is a free version, which is allowed to have only 60 records at a time.
<img width="679" alt="screen shot 2017-10-30 at 3 42 04 pm" src="https://user-images.githubusercontent.com/26446985/32155712-309f7b94-bd8d-11e7-924f-91921d62cf0f.png">

2. Open Web Browser, type '**localhost/weatherMap.html**' as URL
3. Ready to have fun on Google Map

### Visualisation

This section attaches some weather data visualisation snapshots that are captured during testing. Hopefully it will give end-user an idea of how it works without setting up the local environment. 

1. High-Level map to view different cities by combined different countries' data set.
<img width="1609" alt="screen shot 2017-10-30 at 4 22 20 pm" src="https://user-images.githubusercontent.com/26446985/32155992-d3dab26e-bd8e-11e7-86b5-a0f8aced18ea.png">

2. View of China testing data set, which was captured at 2017-10-30T01:00:00Z (command refers to Part1-Run section)
<img width="1634" alt="screen shot 2017-10-30 at 3 16 52 pm" src="https://user-images.githubusercontent.com/26446985/32156075-45e36d38-bd8f-11e7-93b3-d89ad2a47f96.png">


3. View of Australia testing data set, which was captured at 2017-10-30T04:06:00Z (command refers to 2nd command in Part1-Run section)
<img width="1632" alt="screen shot 2017-10-30 at 4 16 59 pm" src="https://user-images.githubusercontent.com/26446985/32156079-4a63a526-bd8f-11e7-9d73-66e0f0ceb445.png">

4. Zone-in to Sydney region

<img width="1679" alt="screen shot 2017-10-30 at 4 18 22 pm" src="https://user-images.githubusercontent.com/26446985/32156119-886aff0e-bd8f-11e7-9e7e-47e42c892988.png">

5. Zone-in to Melbourn region

<img width="1680" alt="screen shot 2017-10-30 at 4 23 40 pm" src="https://user-images.githubusercontent.com/26446985/32156120-8be783aa-bd8f-11e7-8058-a257fb8035bd.png">



## Reference 
<url><br>http://www.partow.net/miscellaneous/airportdatabase/
<url><br>https://openweathermap.org/current#geo
<url><br>http://openweathermap.org/weather-data#
<url>><br>https://en.wikipedia.org/wiki/XAMPP
<ur><br>http://openweathermap.org/api/weathermaps

