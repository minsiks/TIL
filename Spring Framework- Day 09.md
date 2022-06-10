# 6/10 Spring Framework Day 09

## SPRINGBOOT

### 1. Form에 파일을 넣어서 보내는 방법

- add.html

```html
<scrip>
$(document).ready(function(){
	
	$('#registerbtn').click(function(){
		
		$('.user').attr({
			'enctype':'multipart/form-data',
			'method':'post',
			'action':'addimpl'
		});
		$('.user').submit();
	});
});
</scrip>
<!-- 인풋 타입=파일 -->
<div class="form-group">
               IMGNAME: <input type="file" class="form-control form-control-item" name="mf">
            </div>


```

- ProductVO.java

```java
//파일을 담을 수 있는 객체 생성
private MultipartFile mf;
```

- Controller.java

```java
@RequestMapping("addimpl")
	public String addimpl(Model m, ProductVO obj) {
        //파일 이름을 뽑아내는 함수
		String imgname = obj.getMf().getOriginalFilename();
		obj.setImgname(imgname);
		try {
			biz.register(obj);
            //파일저장하는 함수
			Util.saveFile(obj.getMf());
		} catch (Exception e) {
			e.printStackTrace();
		}
		return "redirect:detail?id="+obj.getId();
	}
	
```

- com.multi.frame에 Util.java 파일 복사

```java
package com.multi.frame;

import java.io.FileOutputStream;
import org.springframework.web.multipart.MultipartFile;

public class Util {
	public static void saveFile(MultipartFile mf) {
		String dir = "C:\\test\\shop\\src\\main\\resources\\static\\img\\";
		byte [] data;
		String imgname = mf.getOriginalFilename();
		try {
			data = mf.getBytes();
			FileOutputStream fo = 
					new FileOutputStream(dir+imgname);
			fo.write(data);
			fo.close();
		}catch(Exception e) {
			
		}
		
	}
	
}
```

- 옵션 설정 window - preferences
  - General - Workspace
    - Refresh using native hooks or polling 체크
    - apply

### Semi-Project Team 편성

- 2조
  - 안원영
  - 유정아
  - 서예린
