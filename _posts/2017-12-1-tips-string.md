---
title: 每天一点小知识 String类
date: 2017-12-01 16:21:09
categories:
- Java
comments: true
---

String 类是java程序再常用不过的一个JDK级class，那么对于每天都要面对的String我们是否有足够的认识呢？
今天希望通过一些小知识点让大家能够深入掌握String对象的日常处理。

# String的存储结构
String类的每一个实例都使用一个数组（Array）最为底层存储数据结构。数组的类型为基本类型char，明白这一点之后我们应该意识到String类的绝大多数
API操作都是针对char数组的。例如indexOf,toUpperCase,toCharArray；这意味着很多操作会涉及到数组遍历。

# substring源码解读

代码非常简单，主要看最后一行```return (beginIndex == 0) ? this : new String(value, beginIndex, subLen);``` 如果截取下标从0开始则返回当前String对象，否则利用```public String(char value[], int offset, int count)```构造一个指定区间的新String对象。

```java
public String substring(int beginIndex) {
        if (beginIndex < 0) {
            throw new StringIndexOutOfBoundsException(beginIndex);
        }
        int subLen = value.length - beginIndex;
        if (subLen < 0) {
            throw new StringIndexOutOfBoundsException(subLen);
        }
        return (beginIndex == 0) ? this : new String(value, beginIndex, subLen);
    }
```


# indexOf源码解读

作为程序员通常只需要调用indexOf("find")就能得到结果 ，但实际执行的是下面这段代码；"find"是需要查找的对象作为target被传入，
代码不是特别容易阅读，但是我们可以清晰的看到这种操作一定涉及数组遍历，所以如果能给定范围尽量使用多参调用来争取效率。

需要特别说明的是：不是一般程序员想象的直接去查找"find"，而是先找到"f"，再继续往下匹配。为什么这样？可以思考一下！

```java

/**
     * Code shared by String and StringBuffer to do searches. The
     * source is the character array being searched, and the target
     * is the string being searched for.
     *
     * @param   source       the characters being searched.
     * @param   sourceOffset offset of the source string.
     * @param   sourceCount  count of the source string.
     * @param   target       the characters being searched for.
     * @param   targetOffset offset of the target string.
     * @param   targetCount  count of the target string.
     * @param   fromIndex    the index to begin searching from.
     */
    static int indexOf(char[] source, int sourceOffset, int sourceCount,
            char[] target, int targetOffset, int targetCount,
            int fromIndex) {
        if (fromIndex >= sourceCount) {
            return (targetCount == 0 ? sourceCount : -1);
        }
        if (fromIndex < 0) {
            fromIndex = 0;
        }
        if (targetCount == 0) {
            return fromIndex;
        }

        char first = target[targetOffset];
        int max = sourceOffset + (sourceCount - targetCount);

        for (int i = sourceOffset + fromIndex; i <= max; i++) {
            /* Look for first character. */
            if (source[i] != first) {
                while (++i <= max && source[i] != first);
            }

            /* Found first character, now look at the rest of v2 */
            if (i <= max) {
                int j = i + 1;
                int end = j + targetCount - 1;
                for (int k = targetOffset + 1; j < end && source[j]
                        == target[k]; j++, k++);

                if (j == end) {
                    /* Found whole string. */
                    return i - sourceOffset;
                }
            }
        }
        return -1;
    }
```
