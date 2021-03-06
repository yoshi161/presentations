# Part 5 Unit Testing with JUnit

- Unit Testing
- JUnit Overview
- Modern Style JUnit in Eclipse
- Traditional Style JUnit in Eclipse

---

## Unit Testing

- Testing individual method or small pieces of functionality
- Whenever the code is modified, re-run the test cases to verify
that the result is correct

---

## JUnit Overview

JUnit is a unit testing framework for Java programming language.
- Most popular and widely used unit testing framework in Java.
- Easy to learn basics
- Not part of official Java SE
- Integrated with Eclipse and other IDEs

---

## Modern Style JUnit in Eclipse

- Put **@Test** above any zero-arg method
    - Eclipse will prompt to include the JUnit library (import org.junit.*;)
    - Avoid named **Test** for classes because @Test refers to class
- Import other necessary library
    - Use import static org.junit.Assert.*;
    - Use import static org.hamcrest.CoreMatchers.*;
- Test with **assertThat**
```java
     assertThat(someValue, someMathcer);
```
- Right click in code, choose Run As -> JUnit Test

---

## Traditional Style JUnit in Eclipse

- Put **@Test** above any zero-arg method
    - Eclipse will prompt to include the JUnit library (import org.junit.*;)
    - Avoid named **Test** for classes because @Test refers to class
- Import other necessary library
    - Use import static org.junit.Assert.*;
- Test with **assertTrue** , **assertEquals**, etc
```java
     assertTrue(value);
     assertFalse(value);
     assertEquals(value1, value2);
```
- Right click in code, choose Run As -> JUnit Test

---

### Testing Example

There are 2 method (function) need to be unit test:
- reverseString : the function should reverse a string, preserving case
- isPalindrome : the function should return true if the string reads same
from backward and forward, ignoring case differences

---

### Testing Example

Functions implementation are below:
```java
    public class StringUtils {
        public static String reverseString(String s) {
            return(new StringBuilder(s).reverse().toString());
        }
        public static boolean isPalindrome(String s) {
            return(s.equalsIgnoreCase(reverseString(s)));
        }
        private StringUtils() {}
    }
```

---

### Testing Example

JUnit Modern Approach
```java
package com.mitrais.app;
import org.junit.*;
import static org.junit.Assert.*;
import static org.hamcrest.CoreMatchers.*;

/**
 * The StringUtilsModernTest class implements an JUnit
 * using modern approach.
 */
public class StringUtilsModernTest {
    @Test
    public void testReverse(){
        assertThat("oof", is(equalTo(StringUtils.reverseString("foo"))));
        assertThat("rab", is(equalTo(StringUtils.reverseString("bar"))));
        assertThat("!zaB", is(equalTo(StringUtils.reverseString("Baz!"))));
    }
    @Test
    public void testPalindromes(){
        String[] matches = { "a", "aba", "Aba", "abba", "AbBa",
        "abcdeffedcba", "abcdEffedcba" };
        String[] misMatches = { "ax", "axba", "Axba", "abbax", "xAbBa",
        "abcdeffedcdax", "axbcdEffedcda" };
        for(String s: matches) {
            assertThat(StringUtils.isPalindrome(s), is(true));
        }
        for(String s: misMatches) {
            assertThat(StringUtils.isPalindrome(s), is(false));
        }
    }
}
```

---

### Testing Example

JUnit Traditional Approach
```java
package com.mitrais.app;
import static org.junit.Assert.*;
import org.junit.*;

/**
 * The StringUtilsTradTest class implements an JUnit
 * using traditional approach.
 */
public class StringUtilsTradTes {
    @Test
    public void testReverse(){
        assertEquals("oof", StringUtils.reverseString("foo"));
        assertEquals("rab", StringUtils.reverseString("bar"));
        assertEquals("!zaB", StringUtils.reverseString("Baz!"))
    }
    @Test
    public void testPalindromes(){
        String[] matches = { "a", "aba", "Aba", "abba", "AbBa",
        "abcdeffedcba", "abcdEffedcba" };
        String[] misMatches = { "ax", "axba", "Axba", "abbax", "xAbBa",
        "abcdeffedcdax", "axbcdEffedcda" };
        for(String s: matches) {
            assertTrue(StringUtils.isPalindrome(s));
        }
        for(String s: misMatches) {
            assertFalse(StringUtils.isPalindrome(s));
        }
    }
}
```

---

## Questions?

![alt text](images/Question-Mark.png "Question Mark")