# Oauth, OpenID connect, Identity Server

## Giới thiệu chung
Trong bối cảnh ứng dụng hiện đại, việc quản lý danh tính (xác thực) và quyền truy cập (ủy quyền) là vô cùng quan trọng. OAuth 2.0 và OpenID Connect (OIDC) là hai giao thức chuẩn mở giúp giải quyết các thách thức này, cho phép tích hợp an toàn và liền mạch giữa các dịch vụ.

## OAuth 2.0: Giao thức Ủy quyền

### Khái niệm và mục đích
OAuth 2.0 là một **framework ủy quyền** (authorization protocol). Mục đích chính của nó là cho phép một ứng dụng bên thứ ba truy cập vào các tài nguyên được bảo vệ của người dùng trên một dịch vụ khác (ví dụ: API, dữ liệu) mà **không cần người dùng phải chia sẻ trực tiếp mật khẩu** của họ. OAuth 2.0 trả lời câu hỏi: "Người dùng này có thể làm gì?".  

###  Các Thành phần Cốt lõi
OAuth 2.0 định nghĩa bốn vai trò chính :  
- **Resource Owner (Chủ sở hữu tài nguyên)**: Người dùng sở hữu tài nguyên và cấp quyền truy cập.  
- **Client (Ứng dụng khách)**: Ứng dụng bên thứ ba muốn truy cập tài nguyên của Resource Owner thông qua Resource Server.  
- **Authorization Server (Máy chủ ủy quyền)**: hệ thống chịu trách nhiệm xác thực Resource Owner và cấp mã ủy quyền/token cho Client.  
- **Resource Server (Máy chủ tài nguyên)**: Hệ thống lưu trữ và quản lý tài nguyên được bảo vệ, cấp quyền truy cập dựa trên Access Token hợp lệ. 

### Mã thông báo truy cập (Access Token)
`Access Token` là một mảnh dữ liệu đại diện cho quyền ủy quyền mà Client được cấp để truy cập tài nguyên. Nó được cấp bởi Authorization Server và được Client sử dụng để gửi đến Resource Server.  

`Access Token` thường có thời gian tồn tại ngắn và không có định dạng cụ thể, nhưng JWT (JSON Web Token) thường được sử dụng.  

### Các Luồng Cấp quyền (Grant Types) phổ biến
Grant Type (flow) là một tập hợp các bước được định nghĩa trong OAuth 2.0 mà một Client thực hiện để được cấp ủy quyền truy cập tài nguyên.3 OAuth 2.0 cung cấp nhiều loại Grant Type khác nhau, mỗi loại phù hợp với các kịch bản ứng dụng và yêu cầu bảo mật cụ thể
- **Authorization Code Grant (với PKCE)**: 
    - An toàn nhất và được khuyến nghị cho ứng dụng web truyền thống, SPA (Single Page Applications) và ứng dụng di động.
    - Trong luồng này, Authorization Server không trả về `Access Token` trực tiếp. Thay vào đó, nó trả về một `Authorization Code` sử dụng một lần cho Client. Client sau đó sẽ đổi code này lấy Access Token và Refresh Token (nếu yêu cầu) tại Authorization Server thông qua một kênh bảo mật (thường là kết nối trực tiếp giữa server của Client và Authorization Server).Quá trình trao đổi này diễn ra an toàn ở phía máy chủ của Client.

```text
     +----------+
     | Resource |
     |   Owner  |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier      +---------------+
     |         -+----(A)-- & Redirection URI ---->|               |
     |  User-   |                                 | Authorization |
     |  Agent  -+----(B)-- User authenticates --->|     Server    |
     |          |                                 |               |
     |         -+----(C)-- Authorization Code ---<|               |
     +-|----|---+                                 +---------------+
       |    |                                         ^      v
      (A)  (C)                                        |      |
       |    |                                         |      |
       ^    v                                         |      |
     +---------+                                      |      |
     |         |>---(D)-- Authorization Code ---------'      |
     |  Client |          & Redirection URI                  |
     |         |                                             |
     |         |<---(E)----- Access Token -------------------'
     +---------+       (w/ Optional Refresh Token)
```

- **Client Credentials Grant**: 
    - Dùng cho giao tiếp giữa các máy chủ (machine-to-machine) không có tương tác người dùng.
    - Trong trường hợp này, ứng dụng tự xác thực bằng Client ID và Client Secret của chính nó để truy cập tài nguyên 
```text
     +---------+                                  +---------------+
     |         |                                  |               |
     |         |>--(A)- Client Authentication --->| Authorization |
     | Client  |                                  |     Server    |
     |         |<--(B)---- Access Token ---------<|               |
     |         |                                  |               |
     +---------+                                  +---------------+
```

- **Implicit Grant**: 
    - Là luồng đơn giản hóa của **Authorization Code Grant**, trong đó Access Token được trả lại trực tiếp cho Client dưới dạng tham số trong URI chuyển hướng
    - Tuy nhiên, luồng này hiện đã bị phản đối do tiềm ẩn rủi ro rò rỉ token thông qua các kênh không an toàn
```text
     +----------+
     | Resource |
     |  Owner   |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier     +---------------+
     |         -+----(A)-- & Redirection URI --->|               |
     |  User-   |                                | Authorization |
     |  Agent  -|----(B)-- User authenticates -->|     Server    |
     |          |                                |               |
     |          |<---(C)--- Redirection URI ----<|               |
     |          |          with Access Token     +---------------+
     |          |            in Fragment
     |          |                                +---------------+
     |          |----(D)--- Redirection URI ---->|   Web-Hosted  |
     |          |          without Fragment      |     Client    |
     |          |                                |    Resource   |
     |     (F)  |<---(E)------- Script ---------<|               |
     |          |                                +---------------+
     +-|--------+
       |    |
      (A)  (G) Access Token
       |    |
       ^    v
     +---------+
     |         |
     |  Client |
     |         |
     +---------+
```

- **Resource Owner Password Credentials Grant**: 
    - Grant này yêu cầu Client thu thập trực tiếp thông tin đăng nhập (tên người dùng và mật khẩu) của Resource Owner và gửi chúng đến Authorization Server.
    Luồng này chỉ nên được giới hạn cho các Client hoàn toàn đáng tin cậy (ví dụ: ứng dụng của chính nhà cung cấp dịch vụ) và không có cơ chế chuyển hướng, do đó không phù hợp cho các ứng dụng bên thứ ba.
```text
     +----------+
     | Resource |
     |  Owner   |
     |          |
     +----------+
          v
          |    Resource Owner
         (A) Password Credentials
          |
          v
     +---------+                                  +---------------+
     |         |>--(B)---- Resource Owner ------->|               |
     |         |         Password Credentials     | Authorization |
     | Client  |                                  |     Server    |
     |         |<--(C)---- Access Token ---------<|               |
     |         |    (w/ Optional Refresh Token)   |               |
     +---------+                                  +---------------+
```

## OpenID Connect (OIDC): Lớp Xác thực

### Khái niệm và Mục đích
OpenID Connect (OIDC) là một **giao thức xác thực** được xây dựng như một lớp nhận dạng đơn giản trên nền tảng của OAuth 2.0. Mục đích cốt lõi của OIDC là cho phép xác minh danh tính của người dùng cuối. OIDC trả lời câu hỏi: "Người dùng này là ai?". Nó cũng là giải pháp hữu ích cho Đăng nhập một lần (SSO).  

### Mối quan hệ giữa OIDC và OAuth 2.0
OIDC không phải là giao thức độc lập mà được xây dựng trực tiếp trên OAuth 2.0. OAuth 2.0 tập trung vào **ủy quyền**, còn OIDC tập trung vào **xác thực**. Chúng thường hoạt động song song và bổ trợ cho nhau.  

**Sự khác biệt cốt lõi**:
- **OAuth 2.0**: Mục đích chính là **ủy quyền (authorization)**. Nó cho phép một ứng dụng truy cập vào tài nguyên được bảo vệ của người dùng trên một dịch vụ khác mà không cần biết mật khẩu của họ.3 OAuth 2.0 trả lời câu hỏi "Người dùng này có thể làm gì?".5
- **OpenID Connect**: Mục đích chính là **xác thực (authentication)**. Nó xác định danh tính của người dùng đang truy cập vào ứng dụng.4 OIDC trả lời câu hỏi "Người dùng này là ai?".5


### Mã thông báo định danh (ID Token)
`ID Token` là bằng chứng xác thực chính trong OIDC, được định dạng dưới dạng JSON Web Token (JWT). Nó cung cấp thông tin đã được xác thực về người dùng (claims) và được Client tiêu thụ để xác minh danh tính người dùng, chứ không phải để truy cập API. Client phải xác thực  

`ID Token` bằng cách kiểm tra chữ ký, iss (nhà phát hành), aud (đối tượng), exp (thời gian hết hạn) và nonce (chống replay attack).  

## Triển khai với IdentityServer (Duende IdentityServer)
### Giới thiệu
IdentityServer (hiện là Duende IdentityServer) là một framework mã nguồn mở triển khai các tiêu chuẩn OpenID Connect và OAuth 2.0 cho ASP.NET Core. Nó hoạt động như một máy chủ xác thực trung tâm, phát hành các token bảo mật và cung cấp khả năng kiểm soát cao về UI, UX, logic nghiệp vụ và dữ liệu. IdentityServer tuân thủ các tiêu chuẩn và có thể chạy trên nhiều môi trường khác nhau (on-premises, cloud, Docker, Kubernetes).  

### Cấu hình cơ bản
Để triển khai, cần cấu hình IdentityServer bằng cách :  
- **Khai báo API Resources**: Định nghĩa các API cần bảo vệ và các `ApiScope` chi tiết.  
- **Khai báo Identity Resources**: Định nghĩa các thông tin định danh (claims) về người dùng mà Client có thể yêu cầu (ví dụ: `openid`, `profile`, `email`).  
- **Đăng ký Clients**: Mỗi ứng dụng khách cần được đăng ký với IdentityServer với `Client ID`, `Client Secret`, `Allowed Grant Types`, `Redirect URIs`, `Allowed Scopes`.  

### Luồng cấp phát Token và xác thực người dùng
Khi Client yêu cầu truy cập tài nguyên, nó chuyển hướng người dùng đến IdentityServer để xác thực. Sau khi xác thực thành công, IdentityServer cấp `ID Token` và `Access Token` (và `Refresh Token` nếu yêu cầu) cho Client.  
- **Authorization Code Flow với PKCE**: Client chuyển hướng người dùng đến IdentityServer. Sau khi xác thực và đồng ý, IdentityServer trả về `Authorization Code`. Client sau đó đổi code này lấy `ID Token`, `Access Token` và `Refresh Token` từ IdentityServer qua kênh bảo mật.  
- **Sử dụng Token**: `ID Token` được Client dùng để xác minh danh tính người dùng.  
`Access Token` được Client gửi đến Resource API để truy cập tài nguyên.  
- **Quản lý Refresh Token**: Client có thể yêu cầu `Refresh Token` bằng cách thêm `offline_access` scope. `Refresh Token` được dùng để yêu cầu `Access Token` mới khi token hiện tại hết hạn, không cần tương tác người dùng. IdentityServer hỗ trợ xoay vòng  
- `Refresh Token` để tăng cường bảo mật.  

## Thực tiễn Tốt nhất và Lưu ý Bảo mật
### Bảo mật OAuth 2.0 và OpenID Connect
- **Sử dụng Grant Type an toàn**: Luôn ưu tiên `Authorization Code Flow` với PKCE cho các ứng dụng công khai. Tránh `Implicit Flow` và `Resource Owner Password Credentials Grant`.  
- **Bảo vệ Client Credentials**: `Client Secret` phải được bảo vệ nghiêm ngặt, không lưu trữ trong mã nguồn công khai. Sử dụng mật mã bất đối xứng (asymmetric cryptography) như mTLS hoặc JWTs đã ký cho xác thực client.  
- **Xác thực ID Token đúng cách**: Luôn xác thực chữ ký, `iss`, `aud`, `exp`, và `nonce` của `ID Token` ở phía máy chủ của Client.  
- **Quản lý vòng đời Token**: Cấp `Access Token` với thời gian sống ngắn. Triển khai xoay vòng `Refresh Token` và thu hồi tất cả token khi người dùng đăng xuất.  
- **Nguyên tắc quyền hạn tối thiểu**: Yêu cầu và cấp các `scopes` càng chi tiết càng tốt, chỉ cấp quyền truy cập vào những tài nguyên thực sự cần thiết.  
- **Sử dụng HTTPS/TLS**: Toàn bộ giao tiếp phải diễn ra qua HTTPS/TLS.  
- **Chống giả mạo yêu cầu (CSRF)**: Sử dụng `state` parameter trong yêu cầu ủy quyền.  
- **Triển khai Proof of Possession (DPoP)**: Ràng buộc `Access Token` với Client được ủy quyền để ngăn chặn đánh cắp và phát lại token.  
- **Tuân thủ chính sách**: Đảm bảo tuân thủ các quy định như GDPR, HIPAA, SOX, PCI DSS.  

### Tối ưu hiệu suất và Trải nghiệm người dùng
- **Đăng nhập một lần (SSO)**: OIDC hỗ trợ SSO, cho phép người dùng đăng nhập một lần và truy cập nhiều ứng dụng.  
- **Xác thực không mật khẩu**: IdentityServer có thể hỗ trợ các phương thức như sinh trắc học, OTP, liên kết với Google/Facebook ID.  
- **Quản lý phiên hiệu quả**: Sử dụng `Refresh Token` để duy trì phiên truy cập dài hạn an toàn.  
- **Tùy chỉnh giao diện người dùng**: IdentityServer cho phép tùy chỉnh UI/UX của trang đăng nhập để duy trì nhận diện thương hiệu.  
- **Khả năng mở rộng**: Triển khai IdentityServer dưới dạng không trạng thái (stateless) và trên đám mây để đáp ứng nhu cầu tăng lên. 
 
## Trường hợp nghiên cứu thực tế
- **Đăng nhập bằng Google/Facebook**: Khi người dùng chọn "Đăng nhập bằng Google" hoặc "Đăng nhập bằng Facebook" trên ứng dụng bên thứ ba (ví dụ: Spotify, LinkedIn), họ đang sử dụng OpenID Connect. OP (Google/Facebook) xác thực người dùng và gửi ID Token chứa thông tin hồ sơ cơ bản cho ứng dụng bên thứ ba.  
- **Triển khai Single Sign-On (SSO) trong doanh nghiệp**: Các công ty như Microsoft sử dụng OIDC và OAuth 2.0 để cung cấp SSO cho nhân viên, cho phép họ truy cập nhiều ứng dụng doanh nghiệp chỉ với một lần đăng nhập. Identity Server đóng vai trò là nhà cung cấp danh tính trung tâm.  