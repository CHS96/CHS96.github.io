---
title:  "URI vs URL vs URN"
excerpt: "URI vs URL vs URN을 알아보자!"

categories:
  - Web
  
last_modified_at: 2021-03-12T18:35:00
---

URI는 **Uniform Resource Identifier의 약자로써 인터넷의 자원을 나타내는 고유 식별자**를 의미한다. 즉, 인터넷에 있는 모든 자원을 식별하기 위한 문자열의 구성을 뜻한다.  

나의 블로그 주소인 https://blog.naver.com/djena5078 문자열도 나의 블로그를 식별하는 URI이다.  

URI와 비슷한 개념으로 URL과 URN이 존재한다.  

URI는 인터넷에 있는 모든 자원을 식별하기 위한 고유 식별자라면 URL과 URN은 URI의 하위개념으로 이들의 관계를 그림으로 나타내면 다음과 같다.  

![URI vs URL vs URN](https://user-images.githubusercontent.com/53072057/110897140-42651b00-8340-11eb-97e7-0e919a16e0d1.JPG)  

URL은 **Uniform Resource Locator의 약자로써 URI 내에서 자원의 위치를 뜻하는 Locator​**를 의미한다. 즉, 인터넷상의 어떤 자원을 식별할 때 위치로 자원을 식별하는 부분이다.  

![URI vs URL vs URN1](https://user-images.githubusercontent.com/53072057/110897143-42fdb180-8340-11eb-919e-79f6409acf01.JPG)  

예를 들어 나의 블로그 글 중 하나의 URI인 https://blog.naver.com/djena5078/222269537407에서 URL은 나의 블로그 위치를 나타내는 부분이다.  

![URI vs URL vs URN2](https://user-images.githubusercontent.com/53072057/110897145-43964800-8340-11eb-8bbd-31863e489b64.JPG)  

뒤의 숫자는 현재 글이 블로그에서 몇 번째 글인지를 식별하기 위해 사용되는 문자열이기 때문에 URL이라기보다는 URI에 더욱 적합하다.  

![URI vs URL vs URN3](https://user-images.githubusercontent.com/53072057/110897146-442ede80-8340-11eb-8c5c-3b943599c724.JPG)  

*****

URN은 **Uniform Resource Name의 약자로써 URI 내에서 자원의 이름을 뜻하는 Name**을 의미한다. 즉, 인터넷상의 어떤 자원을 식별할 때 이름으로 자원을 식별하는 부분이다.  

여기서 자원의 이름은 특정 자원을 얻을 수 있는 위치 정보를 포함하지 않고 자원의 위치에 상관없이 고유한 이름만으로 특정 자원을 식별하려는 목적을 가진다.  

![URI vs URL vs URN4](https://user-images.githubusercontent.com/53072057/110897148-442ede80-8340-11eb-9f30-20388b02266d.JPG)  

URL는 자원의 위치를 나타내기 때문에 만약 자원의 위치, URL이 변경된다면 변경 전의 URL로는 해당 자원에 접근할 수 없게 된다.  

하지만 URN은 자원의 위치를 변경하더라도 위치에 영향을 받지 않기 때문에 자원이 이름을 변경하지 않는다면 언제든지 접근할 수 있다는 장점이 있다.  

![URI vs URL vs URN5](https://user-images.githubusercontent.com/53072057/110897150-44c77500-8340-11eb-9ef7-91ec9a60e428.JPG)  


<h2>[ Reference ]</h2>  
* <https://juyeop.tistory.com/48>  

