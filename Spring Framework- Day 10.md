# 6/13 Spring Framework Day 10

## SPRINGBOOT

### 수업내용

1. shop 에 static폴더에 img 폴더 만드릭
2. uitl.java에 추가

```java
package com.multi.frame;

import java.io.FileOutputStream;
import org.springframework.web.multipart.MultipartFile;

public class Util {
	public static void saveFile(MultipartFile mf) {
		String dir = "C:\\spring\\shopadmin\\src\\main\\resources\\static\\img\\";
		String dir2 = "C:\\spring\\shop\\src\\main\\resources\\static\\img\\";
		byte [] data;
		String imgname = mf.getOriginalFilename();
		try {
			data = mf.getBytes();
			FileOutputStream fo = 
					new FileOutputStream(dir+imgname);
			fo.write(data);
			fo.close();
			data = mf.getBytes();
			FileOutputStream fo2 = 
					new FileOutputStream(dir2+imgname);
			fo.write(data);
			fo.close();
		}catch(Exception e) {
			
		}
		
	}
	
}
```

3. detail.html에 추가

```html
<div class ="form-group">
    <img th:src="@{'/img/'+${dp.imgname}}">
</div>
```

4. 히든도 추가해야한다.