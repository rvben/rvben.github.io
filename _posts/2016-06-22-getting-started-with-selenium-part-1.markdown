---
layout: post
title:  "Getting Started with Selenium - Part 1"
date:   2016-06-22 20:00:00 +0200
categories: selenium automated testing junit
---
### Requirements ###
- [Maven](https://maven.apache.org/download.cgi)
- [Git](https://git-scm.com/)
- [IDE of choice](https://www.jetbrains.com/idea/download/)

To get started clone the repository.
```
git clone https://github.com/RJongejan/selenium-getting-started
``` 
When you open the fresh local repository, you'll see the following structure of files and folders:

Regular code:  
src/main/java

Test code:  
src/test/java

**POM**

In the src folder, we find a file named 'pom.xml'. POM stands for 'Project Object Model'. It is an xml-representation of the project. In it, various properties are defined, including the libraries that are used in the project.  
Great about Maven is, that it can automatically get the defined libraries for use in your project. That way, you don't have to make a mess by manually adding jar's to the classpath and keeping local libraries up to date. Maven does all that for us.


**src/test/java**  
The folder where we will keep our test-code. By keeping the test code in a seperate place, we make sure that the test code will not be included in the final product.


**The first test**  
Open the file: myFirstTest.java.

You'll see the following lines, and we'll look at what each one does.

```java
//This line initializes the WebDriver
WebDriver driver = new ChromeDriver();

//This line gives the WebDriver the command to navigate to the Google homepage.
driver.get("http://google.com");

//Our first assertion. Let's see if we are on the Google HomePage.
assertThat("Title is 'Google'.", driver.getTitle(), equalTo("Google"));

//This line defines by what the WebDriver will need to search the Element, as we'll see below.
By finder = By.name("q");

//This line retrieves the WebElement from the page.
WebElement searchField = driver.findElement(finder);

//The 'sendKeys' method of the WebElement is used 
//to type the query in the search-field. 
//We'll add \n to press Enter as well.
searchField.sendKeys("First test.\n");

//Now it's time for an assertion. 
//This will assert if the title is what we expect it to be.
assertThat(driver.getTitle(), containsString("First test. - Google"));
```

When you run this test. And you already may have. You'll see that the last assertion probably fails. That's good, because it brings us to our next subject.  
These days, webpages are designed to be dynamic. In olden days, when dealing with static pages, testing with Selenium was much easier. But then again, static pages might not be as interesting test subject than their dynamic counterparts.  
When dealing with dynamic pages, you'll find that your script might be running faster than the page is loading or changing. That's what we encountered with our last assertion. The page is loading the results, but the assertion is already checking the title. It fails, because the page isn't ready. But Selenium doesn't know it isn't ready yet. It just does what it's asked, and that is to act, not wait. But what if we want it to wait?  

## Wait, WebDriver. Wait! ##
In the WebDriver world, there are different kind of waits. You can wait for a certain amount of time (implicit wait), or you can wait for something to have happened on the page (explicit wait).
