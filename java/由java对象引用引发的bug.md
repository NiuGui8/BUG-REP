项目中用到类似以下代码：

		...
		User user = new User();
		User u = user;
		u.setId(000);
		...

结果user的Id也变为000，原因是 ‘= ’ 实现的仅仅是引用操作，即把右边的地址值赋给左边，所以，u 原本指向 `new User()` 的地址值，后来经过 `u = user` 指向的变成了 user 指向的地址值， 所以 u 的设置操作其实改变的是 user 指向的对象。如果要实现赋值操作需要，就需要类实现Cloneable接口，实现clone()方法。

	class User implements Cloneable{//实现Cloneable接口
		String sex;
		User(String sex){
			this.sex=sex;
		}
		@Override
		protected Object clone() throws CloneNotSupportedException {
			// 实现clone方法
			return super.clone();
		}
	}