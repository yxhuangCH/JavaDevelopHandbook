# JavaDevelopHandbook
阿里巴巴 Java 开发手册的备份

一、编程规约
（一） 命名规约
	6.【强制】抽象类命名使用 Abstract 或 Base 开通；
			  异常类命名使用 Exception 结尾；
			  测试类命名以它要测试类的名称开始，以 Test 结尾。

	7.【强制】中括号是数组类型的一部分，数组定义如下：String[] args;

	8.【强制】POJO 类中布尔类型的变量，都不要加 is, 否则部分框架解析会引起序列化错误。
		反例：定义为基本类型 boolean isSuccess 的属性，它的方法也是 isSuccess(), RPC 框架
			  在方向解析的时候，“以为”对应的属性名称是 success, 导致属性获取不到，进而抛出
			  异常。

	11.【推荐】如果使用到了设计模式，建议在类名中体现出具体模式。
		说明：将设计模式体现在名字中，有利于阅读者快速理解架构设计思想。
		正例：public class OrderFactory;
			  public class LoginProxy;
			  public class ResourceObserver;

	12.【推荐】接口类中的方法和属性不要加任何修饰符号（public 也不要加), 保持代码的整洁
		性，并加上有效的 Javadoc 注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定
		是与接口方法相关，并且是整个应用的基础常量。
		正例：接口方法签名：voidf();
			  接口基础常量表示：String COMPANY = "alibaba";
	    反例：接口方法定义：public abstract void f();
	    说明：JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的
	    	  默认实现。

 （二）常量定义
 	 2.【强制】long 或者 Long 初始赋值时，必须使用大写的 L, 不能是小写的 l,小写容易跟数字
 	 	1 混淆，造成误解。

 	 3.【推荐】不要使用一个常量类维护所有常量，应该按照常量功能进行归类，分开维护。
 	 	如：缓存相关的常量放在类 CacheConsts 下，系统配置相关的常量放在类 ConfigConsts 下。
 	 	说明：大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。

 	 5.【推荐】如果变量值仅在一个范围内变化用 Enum 类。如果还带有名称之外的延伸属性，必须使用
 	 	Enum 类，下面正例中的数据就是延伸信息，表示星期几。
 	 	正例：public Enum{
		 		MONDAY(1),
		 		TUESDAY(2),
		 		WEDNESDAY(3),
		 		THURSDAY(4),
		 		FRIDAY(5),
		 		SATURDAY(6),
		 		SUNDAY(7);
		 	}

	(三)格式规约
		1.【强制】大括号的使用约定。
		   如果大括号内为空，则简洁地写成{}即可，不需要换行；

		2.【强制】左括号和后一个字符之间不出现空格；同样，右括号和前一个字符之间也不出现空格。

		3.【强制】if/for/while/swithc/do 等保留字与左右括号都必须加空格。

		4.【强制】任何运算符号左右必须加一个空格。
		   说明：运算符包括赋值运算=、逻辑运算符&&、加减乘除符号、三目运行符等。

		5.【强制】缩进采用 4 个空格，禁止使用 tab 字符。
		   说明：如果使用 tab 缩进，必须设置 1 个 tab 为 4 个空格。 IDEA 设置 tab 为 4 个空格时，
		   请勿勾选 Use tab character; 而在 eclipse 中，必须勾选 insert spaces for tabs.

		   正例：
		   		public static void main(String args[]){
		   			// 缩进 4 个空格
		   			String say = "hello";
		   			// 运算符的左右必须有一个空格
		   			int flag = 0;
		   			// 关键词 if 与括号之间必须有一个空格，括号内的 f 与左括号， 0 与右括号不需要空格
		   			if (flag == 0){
		   				System.out.println(say);
		   			}

		   			// 左大括号前加空格且不换行；左大括号后换行
		   			if (flag == 1){
		   				System.out.prinln("world");
		   				// 右大括号前换行，右大括号后有 else, 不用换行
		   			} else {
		   				System.out.println("ok");
		   				// 在右大括号后直接结束，则必须换行
		   			}
		   		}

	（四）OOP 规约
		1.【强制】避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加编译器解析成本，
		   应该直接用类名访问。

		3.【强制】相同参数类型，相同业务含义，才可以使用 Java 的可变参数，避免使用 Oject.
		   说明：可变参数必须放置在参数列表的最后。（提倡同学们尽量不用可变参数）
		   正例：public User getUsers(String type, Integer...ids)
		
		4.【强制】对外暴露的接口签名，原则上不允许修改方法签名，避免对接口调用方产生影响。接口过时
		   必须加 @Deprecated 注解，并清晰的说明采用的新接口或者新服务是什么。

		5.【强制】不要使用过时的类或方法。

		6.【强制】Oject 的 equals 方法容易抛空指针异常，应使用常量或确定有值的对象类调用 equals.
		   正例：“test”.equals(object);
		   反例：object.equals("test");
		   说明：推荐使用 java.util.Ojects#equals(JDK 7 引入的工具类)
		
		7.【强制】所有的相同类型的包装类对象之间值的比较，全部使用 equals 方法比较。
	       说明：对于 Integer var=? 在 -128 至 127 之间赋值，Integer 对象是在 IntegerCache.cache
	       产生，会复用已有对象，这个区间内的 Integer 值可以直接使用 == 进行判断，但是这个区间之外
	       的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用 equals 方法进行判断。

	    11.【强制】构造方法里面禁止加入任何业务逻辑，如果有初始化逻辑，请放在 init 方法中。

	    12.【强制】POJO 类必须写 toString 方法。如果继承了另一个 POJO 类，注意在前面加一下 super.toString.
			说明：在方法执行抛出异常时，可以直接调用 POJO 的方法打印其属性，便于排查问题。

		13.【推荐】使用索引访问 String 的 split 方法得到的数组时，需做最后一个分隔符有无内容的检查，否则
			会有抛 IndexOutOfBoundsException 的风险。
			说明：String str = "a,b,c,,";
				  String[] ary = str.split(",");
				  // 预期大于 3，结果是 3
				  System.out.println(ary.length);

	    14.【推荐】当一个类有多个构造方法，或者有多个同名方法，这些方法应该按顺序放置在一起，便于阅读。

	    15.【推荐】类内方法定义顺序依次是: 公有方法或保护方法 > 私有方法 > getter/setter 方法。

	    16.【推荐】setter 方法中，参数名称与类成员变量名称一致， this.成员名 = 参数名。在 getter/setter
	    	方法中，尽量不要增加业务逻辑，增加排查问题的难度。

	    17.【推荐】循环体内，字符串的串接方式，使用 StringBuilder 的 append 方法进行扩展。
	    	反例：
	    		String str = "start";
	    		for (int i = 0; i < 100; i++){
	    			str = str + "hello";
	    		}
	    	说明：反编译出的字节码文件显示每次循环都会 new 出一个 StringBuilder 对象，然后进行 append 操作，
	    		  最后通过 toString 方法返回 String 对象，造成内存资源浪费。

	    18.【推荐】final 可提高程序响应效率，声明称 final 的情况：
	    	. 不需要从新赋值的变量，包括类属性、局部变量；
	    	. 对象参数前加 final， 表示不允许修改引用的指向。
	    	. 类方法确定不允许被重写。

	    19.【推荐】慎用 Oject 的 clone 方法来拷贝对象。
	    	说明：对象的 clone 方法默认是浅拷贝，若想实现深拷贝需要重写 clone 方法实现属性对象的拷贝。

	（五）集合处理
		7.【强制】不要在 foreach 循环里进行元素的 remove/add 操作。 remove 元素使用 Iterator 方式，
		   如果并发操作，需要对 Iterator 对象加锁。
		   反例：
		   		List<String> a = new ArrayList<String>();
		   		a.add("1");
		   		a.add("2");
		   		for (String temp : a){
		   			if ("1".equals(temp)){
		   				a.remove(temp);
		   			}
		   		}
		   	正例：
		   		 Iterator<String> it = a.iterator();
		   		 while (it.hasNext()){
		   		 	String temp = it.next();
		   		 	if (删除元素的条件){
		   		 		it.remove();
		   		 	}
		   		 }




