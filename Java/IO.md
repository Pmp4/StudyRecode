# 🔌 JAVA IO(Input/Output) 🔌
IO는 입출력을 뜻하며, <b>내부</b> 또는 <b>외부</b>의 장치와 프로그램간의 데이터를 주고 받는 행위를 말한다.

자바에서 IO을 수행하기 위해선 <b>Stream</b>을 사용해야 하는데, 대상끼리의 데이터를 운반하는 통로를 말한다.
```
Data Source => InputStream => Java Programme => OutputStream => 목적지
```

## 🛤 Stream
### 종류
- 데이터에 따른 구분


직렬화
객체를 네트워크를 통해 전송, 파일로 저장할 때 직렬화를 해야한다
인스턴스 변수들의 값을 일렬로 나열하는 것

역직렬화


스트림
직렬화하여 입출력할 수 있게 해주는 보조스트림
ObjectInputStream
ObjectOutputStream
