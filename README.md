Html Elements framework
=======================
This framework is designed to provide an easy-to-use way of interacting with web-page elements in your tests.
It can be considered to be an extension of WebDriver Page Object.

With the help of the Html Elements framework you can group web-page elements into blocks, encapsulate logic of interaction within them 
and then easily use created blocks in page objects. It also provides a set of helpful matchers to use with web-page elements
and blocks. See [Samples](https://github.com/yandex-qatools/htmlelements-examples) for more details.

You can ask your questions on StackOverflow with the [![htmlelements](https://img.shields.io/badge/stackoverflow-htmlelements-orange.svg?style=flat)](http://stackoverflow.com/questions/tagged/htmlelements) tag.

Release Notes
-------------

* [Version 1.21 Release Notes](https://github.com/Zlaman/htmlelements-java/releases/tag/htmlelements-1.21)
* [Version 1.19 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.19)
* [Version 1.18 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.18)
* [Version 1.17 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.17)
* [Version 1.16 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.16)
* [Version 1.15 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.15)
* [Version 1.14 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.14)
* [Version 1.13 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.13)
* [Version 1.12 Release Notes](https://github.com/yandex-qatools/htmlelements/releases/tag/htmlelements-1.12)
* [Version 1.11 Release Notes](https://github.com/yandex-qatools/htmlelements/blob/master/releasenotes/1.11-releasenotes.ru.md)
* [Version 1.10 Release Notes](https://github.com/yandex-qatools/htmlelements/blob/master/releasenotes/1.10-releasenotes.ru.md)
* [Version 1.9 Release Notes](https://github.com/yandex-qatools/htmlelements/blob/master/releasenotes/1.9-releasenotes.ru.md)

Include Html Elements in your project
-------------------------------------
Maven dependencies for Html Elements core:

```xml
<dependency>
    <groupId>ru.yandex.qatools.htmlelements</groupId>
    <artifactId>htmlelements-java</artifactId>
    <version>1.21.0-SNAPSHOT</version>
</dependency>
```

Since 1.21 Java 17 is required. Please use 1.20 for Java 8 support.
Since 1.15 Java 8 is required. Please use 1.14 for Java 7 support.

Create blocks of elements
-------------------------
For example, let's create a block for the search form on the page http://www.yandex.com:

```java
@Name("Search form")
@FindBy(xpath = "//form")
public class SearchArrow extends HtmlElement {

    @Name("Search request input")
    @FindBy(id = "searchInput")
    private TextInput requestInput;

    @Name("Search button")
    @FindBy(className = "b-form-button__input")
    private Button searchButton;

    public void search(String request) {
        requestInput.sendKeys(request);
        searchButton.click();
    }
}
```

Construct page object using created blocks
------------------------------------------
You can easily use created blocks in page objects:

```java
public class SearchPage {

    private SearchArrow searchArrow;
    // Other blocks and elements here ...

    public SearchPage(WebDriver driver) {
        PageFactory.initElements(new HtmlElementDecorator(new HtmlElementLocatorFactory(driver)), this);
    }

    public void search(String request) {
        searchArrow.search(request);
    }

    // Other methods here ...
}
```

Use page objects in your tests
------------------------------
Created page objects can be used in your tests. This makes tests more concise, easier to maintain, and easy to write.

```java
public class SampleTest {

    private WebDriver driver = new FirefoxDriver();
    private SearchPage searchPage = new SearchPage(driver);

    @Before
    public void loadPage() {
        driver.get("http://www.yandex.com");
    }

    @Test
    public void sampleTest() {
        searchPage.search("yandex");
        // Some assertion here
    }

    @After
    public void closeDriver() {
        driver.quit();
    }
}
```

Questions?
----------
In case you can't find an answer in documentation and examples provided above, you can ask it on StackOverflow with the 
[![htmlelements](https://img.shields.io/badge/stackoverflow-htmlelements-orange.svg?style=flat)](http://stackoverflow.com/questions/tagged/htmlelements) tag.
