阅读jdk源码时经常发现类有一个serialVersionUID的字段；该字段用于在反序列
化时，判断源类和当前类是否版本一致；不一致会抛出InvalidCastException异常；
因此，带有该字段的类都实现了Serializable接口；

serialVersionUID的生成，根据包名，类名，继承关系，非私有的方法和属性，以
及参数，返回值等诸多因子计算得出的，极度复杂生成的一个64位的哈希字段。
计算出来的这个值是唯一的，可以认为serialVersionUID是对该类综合信息的摘要；  

当序列化和反序列化时，只要二者serialVersionUID一致，不管两者类是否真的一
致，都会执行反序列化，二者不同的字段置为默认值；  

如果子类实现了Serializable接口，而父类没有实现该接口，那么在反序列化时，
就无法从序列化的值中构建出父类对象，默认的处理是调用父类的无参构造函数；
如果父类不存在无参构造函数，就会抛出异常；