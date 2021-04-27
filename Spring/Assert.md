## Assert(断言)



[toc]

spring的一个抽象工具类

```java
package org.springframework.util
    
public abstract class Assert{
    
}

```

##### 方法(不通过则抛出 IllegalStateException)

###### state（判断结果为false，抛出异常）

```java
	public static void state(boolean expression, String message) {
		if (!expression) {
			throw new IllegalStateException(message);
		}
	}
```

###### isTrue（判断结果为false，抛出异常）

```java
	public static void isTrue(boolean expression, String message) {
		if (!expression) {
			throw new IllegalArgumentException(message);
		}
	}
```

###### isNull（不为空，抛出异常）

```java
	public static void isNull(@Nullable Object object, String message) {
		if (object != null) {
			throw new IllegalArgumentException(message);
		}
	}
```

###### notNull（为null，抛出异常）

###### hasLength（长度为0，抛出异常）

###### hasText（只有空格、、、，无文本，抛出异常）

###### doesNotContain（textToSearch包含substring,抛出异常）

```java
public static void doesNotContain(@Nullable String textToSearch, String substring, String message) {
    if (StringUtils.hasLength(textToSearch) && StringUtils.hasLength(substring) &&
        textToSearch.contains(substring)) {
        throw new IllegalArgumentException(message);
    }
}
```

###### notEmpty（为空，抛出异常）

###### noNullElements（有空元素，抛出异常）