---
title: "Kiểm thử code mạng Java với JUnit & WireMock"
date: 2025-10-19
tags: ["Java", "Testing", "WireMock", "JUnit"]
categories: ["Java"]
draft: false
cover: "images/posts/kiem-thu-network-java-junit-wiremock.png"
---


Mock server HTTP, giả lập tình huống lỗi, timeouts, và viết test đáng tin cậy cho logic mạng.

**Từ khoá:** Java, Testing, WireMock, JUnit.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Vì sao cần mock?
Trong lập trình mạng, code thường tương tác với server, API, hoặc socket, nên cần kiểm tra kỹ lưỡng. JUnit là framework kiểm thử đơn vị (unit testing) phổ biến trong Java, còn WireMock là công cụ giả lập (mocking) để mô phỏng phản hồi từ server mạng – rất hữu ích khi không muốn phụ thuộc vào server thật. Kiểm thử code mạng **không phụ thuộc** hệ thống bên ngoài. `WireMock` giúp giả lập API với scenario, delay, lỗi…

Bắt đầu với các bước cơ bản:
- **Cài đặt JUnit**: Thêm thư viện vào project (qua Maven hoặc Gradle), học cách viết test case với @Test.
- **Hiểu WireMock**: Tải WireMock (dùng JAR hoặc Docker), cấu hình stub để giả lập API hoặc socket.
- **Kiểm thử mạng**: Viết test cho các tác vụ như gọi HTTP, socket, hoặc xử lý ngoại lệ.
Dùng JUnit để kiểm tra logic, và WireMock để giả lập phản hồi từ server. 

## 2) Ví dụ test

WeatherService.java (Lớp cần kiểm thử) 

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class WeatherService {
    public String getWeather(String city) throws Exception {
        URL url = new URL("http://localhost:8080/weather/" + city);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String line = reader.readLine();
        reader.close();
        conn.disconnect();
        return line;
    }
}
```

WeatherServiceTest.java (Test với JUnit & WireMock)

```js
import org.junit.Before;
import org.junit.Rule;
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import com.github.tomakehurst.wiremock.junit.WireMockRule;

public class WeatherServiceTest {
    @Rule
    public WireMockRule wireMockRule = new WireMockRule(8080);

    @Before
    public void setUp() {
        wireMockRule.stubFor(get(urlEqualTo("/weather/Hanoi"))
            .willReturn(aResponse()
                .withStatus(200)
                .withBody("25°C")));
    }

    @Test
    public void testGetWeatherSuccess() throws Exception {
        WeatherService service = new WeatherService();
        String result = service.getWeather("Hanoi");
        assertEquals("25°C", result);
    }

    @Test(expected = Exception.class)
    public void testGetWeatherError() throws Exception {
        wireMockRule.stubFor(get(urlEqualTo("/weather/ErrorCity"))
            .willReturn(aResponse()
                .withStatus(500)
                .withBody("Server error")));
        WeatherService service = new WeatherService();
        service.getWeather("ErrorCity"); // Kiểm tra ngoại lệ
    }
}
```
Cách chạy:

1.Thêm dependency WireMock trong pom.xml:
```js
<dependency>
    <groupId>com.github.tomakehurst</groupId>
    <artifactId>wiremock</artifactId>
    <version>2.35.0</version>
    <scope>test</scope>
</dependency>
```

2.Compile và chạy test: mvn test hoặc chạy trong IDE.
3.Xem kết quả: Hai test (testGetWeatherSuccess và testGetWeatherError) sẽ pass nếu WireMock trả về đúng dữ liệu và ngoại lệ!
Lưu ý: Cần cài Maven và WireMock trước.

## Best-practices nhanh
- Ghi log có cấu trúc (JSON) + traceId.
- Timeout hợp lý cho connect/read/write; tổng timeout (deadline) khi cần.
- Chuẩn hoá lỗi cho client; đừng lộ stacktrace ra ngoài.
- Viết kiểm thử với trường hợp **lỗi** (timeout, 429, 5xx), không chỉ thành công.

## Checklist áp dụng
- [ ] Thiết kế protocol hoặc schema API rõ ràng.
- [ ] Thêm retry/backoff có điều kiện.
- [ ] Quan sát (metrics/log/trace) đầy đủ.
- [ ] Tài liệu hoá ví dụ request/response (hoặc mock contract).

## Kết luận
Những nguyên tắc ở trên đủ để bạn triển khai và mở rộng trong bối cảnh dự án thực tế. 

- Kỹ năng kiểm thử: Nắm cách viết test case, dùng assert, và kiểm tra ngoại lệ.
- Hiểu mạng sâu hơn: Hiểu cách dữ liệu di chuyển qua HTTP/socket, và cách mock phản hồi.
- Tối ưu code: Phát hiện lỗi sớm (như timeout, null pointer) trước khi deploy.
- Chuẩn bị nghề nghiệp: Đây là kỹ năng cần thiết cho QA hoặc dev trong dự án mạng.
- Tư duy phòng thủ: Học cách nghĩ đến mọi kịch bản (thành công, lỗi) khi viết code.
