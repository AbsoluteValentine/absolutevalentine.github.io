# 关于软件验证中的单元测试


##### 为什么使用单元测试

一般来说，验证的两个思路是证明和证伪，分别对应着软件验证的形式化验证和测试。

证明适用于在有明确的逻辑范畴内通过演绎推理（如三段论、假言推理等）进行；证伪可通过举出反例的方式进行。

软件领域的性质导致证明的困难及收效甚微，绝大部分都采用测试来验证。

根据测试目的的不同有功能测试、性能测试等；根据是否了解测试对象分为黑盒测试与白盒测试，单元测试属于白盒测试。

一般来说，开发人员了解代码的结构，自己走一遍流程或CodeReview等形式基本可以发现bug，但为了团队中其他人员及日后的维护，单元测试有一定的存在的合理性。

因此需要保持单元测试中方法的独立性，隔离测试依赖，调用其他方法时需要模拟；给定输入，执行测试，验证输出，需要用到断言。


##### JAVA常用测试框架junit、mockito、powermock

###### junit常用注解

``` java
@Ignore                                该注解标记的测试方法在测试中会被忽略
@Test(expected=xxxException.class)     断言该方法会抛出异常
@Test(timeout=1000)                    执行时间超过设置的值该案例会失败
@RunWith(JUnit4.class)                 默认运行器
@RunWith(PowerMockRunner.class)        PowerMockRunner运行器
@RunWith(Parameterized.class)          参数化运行器
@Rule
public class ExpectedExceptionsTest {
    @Rule
    public ExpectedException thrown = ExpectedException.none();
    @Test
    public void verifiesTypeAndMessage() {
        thrown.expect(RuntimeException.class);
        thrown.expectMessage("Runtime exception occurred");
        throw new RuntimeException("Runtime exception occurred");
    }
}
```

###### 参数化测试

``` java
@RunWith(Parameterized.class)
public class Testa {
    @Parameterized.Parameters
    public static Collection<?> data() {
        return Arrays.asList(new Object[][] { { "1+2", 3 }, { "1+2+5", 8 }});
    }
    @InjectMocks
    Calculator calc;
    @Parameterized.Parameter(0)
    public String input;
    @Parameterized.Parameter(1)
    public int expected;
    @Test
    public void testCalculate() {
        int r = calc.calculate(this.input);
        assertEquals(this.expected, r);
    }
}
```

###### 断言

~~~ java
assertEquals(a, b)    测试a是否等于b（a和b是原始类型数值或者必须为实现比较而具有equal方法）
assertFalse(a)        测试a是否为false（假），a是一个Boolean数值。
assertTrue(a)         测试a是否为true（真），a是一个Boolean数值
assertNotNull(a)      测试a是否非空，a是一个对象或者null。
assertNull(a)         测试a是否为null，a是一个对象或者null。
assertNotSame(a, b)   测试a和b是否没有都引用同一个对象。
assertSame(a, b)      测试a和b是否都引用同一个对象。
fail(string)          Fail让测试失败，并给出指定信息。
~~~

###### mockito使用介绍

~~~ java
通过代码创建
public class UserServiceTest {  
    private UserService userService;  
    private UserDao mockUserDao;  
    @Before
    public void setUp() {
    	mockUserDao = mock(UserDao.class);  
    	userService = new UserServiceImpl();  
    	userService.setUserDao(mockUserDao);  
    }
通过注解创建
public class UserServiceTest {  
    @InjectMocks
    private UserServiceImpl userService;  
    @Mock
    private UserDao mockUserDao;   
}
常用方法
verify
    verify(mock, never()).add();       		  验证add方法没有被调用
    verify(mock, times(2)).add();      		  验证add方法被调用了2次
    verify(mock, atLeast(n)).someMethod();        方法至少被调用n次
    verify(mock, atMost(n)).someMethod();         方法最多被调用n次
when
    when(mock.someMethod()).thenReturn(value1).thenReturn(value2);
    when(mock.someMethod()).thenThrow(new RuntimeException());
spy
    List spy = spy(new LinkedList());
    when(spy.get(0)).thenReturn(“foo");
    doReturn("foo").when(spy).get(0);
~~~

###### 使用powermock 测试private方法

~~~ java
@RunWith(PowerMockRunner.class)
//类上加注解 @PrepareForTest({xxx.class})
@PrepareForTest({RpaRightsConfService.class})
public class RpaRightsConfServiceTest {
    @InjectMocks
    private RpaRightsConfService rpaRightsConfService;
    @Test
    public void getCellValue() throws Exception{
        Row row = Mockito.mock(Row.class);
        Cell cell = Mockito.mock(Cell.class);
        cell.setCellType(Cell.CELL_TYPE_STRING);
        Mockito.when(row.getCell(Mockito.anyInt())).thenReturn(cell);
        //通过反射
        Method method1 = rpaRightsConfService.getClass().getDeclaredMethod("getCellValue", Row.class,int.class);
        method1.setAccessible(true);// 抑制访问修饰符，使得私有方法变为可以访问的
        method1.invoke(rpaRightsConfService,row,3);
        Mockito.verify(row, Mockito.times(1)).getCell(Mockito.anyInt());
        //或者通过PowerMockito.method
        Method method2 = PowerMockito.method(RpaRightsConfService.class, "getCellValue", Row.class,int.class);
        Object x2 = method2.invoke(rpaRightsConfService,row,3);
        assertEquals(Integer.parseInt(x2.toString()),0);
    }
}
~~~

###### 使用powermock mock静态方法/final方法/new

~~~java
@RunWith(PowerMockRunner.class)
//类上加注解 @PrepareForTest({xxx.class})
@PrepareForTest({JSON.class,HSSFWorkbook.class})
public class RpaRightsConfServiceTest {
    @InjectMocks
    private RpaRightsConfService rpaRightsConfService;
    @Test
    public void save() {
        //mock 静态
        PowerMockito.mockStatic(JSON.class);
        List<RpaRightsConfVo> rpaRightsConfVoList =new ArrayList<RpaRightsConfVo>();
        rpaRightsConfVoList.add(new RpaRightsConfVo());
        PowerMockito.when(JSON.parseArray(formListJson, RpaRightsConfVo.class)).thenReturn(rpaRightsConfVoList);
        rpaRightsConfService.save();
    }
    @Test
    public void importExcel() throws Exception{
        RpaRightsConfForm form = Mockito.mock(RpaRightsConfForm.class);
        InputStream is = Mockito.mock(InputStream.class);
        FormFile file = Mockito.mock(FormFile.class);
        HSSFWorkbook wb = PowerMockito.mock(HSSFWorkbook.class);
        Mockito.when(form.getFile()).thenReturn(file);
        Mockito.when(file.getInputStream()).thenReturn(is);
        //mock new
        PowerMockito.whenNew(HSSFWorkbook.class).withArguments(is).thenReturn(wb);
        rpaRightsConfService.importExcel(form);
    }
}
~~~
