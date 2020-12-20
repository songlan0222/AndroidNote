单元测试

[toc]

# 本地测试

## 1. 添加依赖

```json
dependencies {
    // Required -- JUnit 4 framework
    testImplementation 'junit:junit:4.12'
    // Optional -- Mockito framework（可选，用于模拟一些依赖对象，以达到隔离依赖的效果）
    testImplementation 'org.mockito:mockito-core:2.19.0'
}
```

## 2. 创建测试类

快捷方式：选择对应的类->将光标停留在类名上->按下ALT + ENTER->在弹出的弹窗中选择Create Test

![img](D:\Program\AndroidNote\_resources\createTestClass)



勾选

- `setUp/@Before`会生成一个带`@Before`注解的`setUp()`空方法

- `tearDown/@After`则会生成一个`带@After`的空方法。



# 设备测试