# JAVA   == 与 equals
```

String a="abc";
String b="abc";
System.out.println(c.equals(b));//true
System.out.println(a==b);//也是true，因为java默认字符串是常量，也就是说a和b的地址（java没有指针，假设是地址）一致
String c=new String("123");
String d=new String("123");
System.out.println(c.equals(d));//true
System.out.println(c==d);//这时应该是false，因为new出来的话会申请不同的地址，而==号就是比较他们的地址（java没有指针，假设是地址）
```


结论：不要用==来判断字符串是否相等，要使用equals方法。