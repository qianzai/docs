# Java反射

```java
import com.ruoyi.common.annotation.Excel;
import com.ruoyi.common.core.domain.entity.SysUser;
import org.junit.Test;

import java.lang.reflect.Field;

public class T1 {

	@Test
	public void test1() throws NoSuchFieldException, IllegalAccessException {

		//获取SysUser的一个对象和类模板
		SysUser sysUser = new SysUser();
		Class<SysUser> sysUserClass = SysUser.class;

		//获取类模板的属性
		Field userId = sysUserClass.getDeclaredField("userId");

		//通过反射设置值
		userId.setAccessible(true);
		userId.set(sysUser, 1L);

		System.out.println("sysUser.getUserId() = " + sysUser.getUserId());

		//获得注解
		Excel annotation = userId.getAnnotation(Excel.class);
		System.out.println("annotation.name() = " + annotation.name());
	}
}
```



