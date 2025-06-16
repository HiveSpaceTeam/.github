# 🛒 Dự Án Hivespace

<!-- > Một dự án phụ để học và áp dụng kiến thức về kiến trúc, thiết kế, cloud và nguyên tắc DevOps, bao gồm microservices, domain-driven design và Azure. Chúng tôi hướng đến việc xây dựng một hệ thống thương mại điện tử được thiết kế cho khả năng mở rộng và thích ứng để giải quyết các thách thức chính trong lĩnh vực thương mại điện tử. -->
<!-- > [🇬🇧 View English version here](./README.md) -->

## 📖 Mục Lục
1. [Tổng Quan](#tổng-quan)
2. [Kiến Trúc](#kiến-trúc)
3. [Cấu Trúc Dự Án](#cấu-trúc-dự-án)
    - [Backend](#backend)
    - [Frontend](#frontend)
4. [DevOps](#devops)
5. [Công Nghệ](#công-nghệ)
6. [Thư Viện & Mẫu Thiết Kế Phổ Biến](#thư-viện--mẫu-thiết-kế-phổ-biến)
7. [Nguồn Cảm Hứng & Tham Khảo](#nguồn-cảm-hứng--tham-khảo)
8. [Đóng Góp & Phản Hồi](#đóng-góp--phản-hồi)

---

## Tổng Quan

Dự án này là một dự án phụ được thiết kế để áp dụng và nâng cao kiến thức về kiến trúc phần mềm, phát triển backend, dịch vụ đám mây và nguyên tắc DevOps. Nó mô phỏng một nền tảng thương mại điện tử đơn giản nhưng thực tế, lấy cảm hứng từ các hệ thống thực tế như Shopee hoặc TiktokShop.

Mặc dù các hệ thống thương mại điện tử khá phổ biến, nhưng việc xây dựng chúng tốt không hề đơn giản. Một nền tảng thương mại điện tử mạnh mẽ cần giải quyết nhiều vấn đề thực tế như:
- Xác thực người dùng và quản lý hồ sơ
- Danh mục sản phẩm với tìm kiếm và bộ lọc
- Giỏ hàng và đặt hàng
<!-- - Quản lý kho và giá
- Mô phỏng thanh toán và vận chuyển
- Bảng điều khiển quản trị cho quản lý sản phẩm và đơn hàng (Đang phát triển)
- Cờ tính năng và công việc nền
- Thiết kế cho khả năng mở rộng và tách biệt dịch vụ -->

Chúng tôi chọn thương mại điện tử vì nó cung cấp một môi trường học tập phong phú có thể phát triển theo thời gian. Bắt đầu với trải nghiệm người mua cốt lõi, chúng tôi hướng đến việc mở rộng dần dần hệ thống để bao gồm các lĩnh vực nâng cao hơn như:
- **Trung Tâm Người Bán** (tính năng đa người bán)
- **Quản Lý Hậu Cần** (theo dõi đơn hàng, lên lịch giao hàng)
- **Tương Tác Khách Hàng** (đánh giá, xếp hạng, đề xuất)
- **Thương Mại Xã Hội** (bảng tin người dùng, mua sắm trực tuyến, hỗ trợ trò chuyện)

Tầm nhìn dài hạn này cho phép chúng tôi:

- Thực hành xây dựng mã nguồn mô-đun, dễ bảo trì
- Áp dụng các mẫu như **Domain-Driven Design (DDD)** và **Clean Architecture**
- Chuyển đổi từ cấu trúc nguyên khối sang **microservices**
- Triển khai các thực hành cloud-native sử dụng các công cụ như **Docker**, **Kubernetes** và **dịch vụ Azure**
- Khám phá **khả năng mở rộng**, **khả năng quan sát**, **CI/CD** và **khả năng phục hồi hệ thống** trong môi trường giống như sản xuất

Cuối cùng, dự án này là một **sân chơi cho việc học tập liên tục**, được xây dựng xung quanh một không gian vấn đề thực tế với ứng dụng rộng rãi trong nhiều ngành công nghiệp.

## Kiến Trúc

Chúng tôi kết hợp nhiều mẫu và phong cách kiến trúc, bao gồm:
- [Domain-Driven Design](../architecture/domain-driven-design.md)
- Kiến Trúc Hướng Sự Kiện
- Command Query Responsibility Segregation (CQRS)

### Tại Sao Bắt Đầu Với Monolith?
1. **Phát Triển Nhanh Hơn**: Xây dựng và kiểm tra tính năng nhanh chóng mà không cần lo lắng về ranh giới dịch vụ hoặc lớp giao tiếp.
2. **Cơ Sở Hạ Tầng Đơn Giản Hơn**: Không cần thiết lập Kubernetes, khám phá dịch vụ, cổng API hoặc theo dõi phân tán ngay lập tức.
3. **Gỡ Lỗi Dễ Dàng Hơn**: Nhật ký, dấu vết ngăn xếp và lỗi đều ở một nơi, giúp dễ dàng hiểu điều gì đang xảy ra.
4. **Giảm Tải Nhận Thức**: Tập trung nhiều hơn vào logic nghiệp vụ và phát triển tính năng thay vì độ phức tạp của thiết kế hệ thống.

### Chuyển Đổi Sang Microservices
Khi ổn định, chúng tôi sẽ chuyển sang microservices để:
- Triển khai mô-đun
- Mở rộng độc lập
- Phân tách mối quan tâm tốt hơn

#### Các Giai Đoạn Di Chuyển:
1. **Kế Hoạch Trích Xuất Dịch Vụ:** Xác định ranh giới miền và trích xuất dịch vụ.
2. **Giao Tiếp:** Triển khai REST, gRPC hoặc giao thức nhắn tin.
3. **Triển Khai:** Sử dụng Docker và Kubernetes cho container hóa và điều phối.

## Cấu Trúc Dự Án

### Frontend
Frontend được xây dựng bằng Vue.js và Vite cho trải nghiệm phát triển nhanh và hiện đại.

### Backend
Dự án backend được lưu trữ trong [repo này](https://github.com/HiveSpaceTeam/hivespace.backend), bao gồm 3 dự án tương ứng với 3 lớp:
1. **Lớp Ứng Dụng:** Xử lý tương tác người dùng và điều phối logic nghiệp vụ.
2. **Lớp Miền:** Chứa logic nghiệp vụ cốt lõi và mô hình miền.
3. **Lớp Cơ Sở Hạ Tầng:** Quản lý cơ sở dữ liệu, nhắn tin và tích hợp bên ngoài.

## DevOps

- **CI/CD:** Jenkins pipelines cho tự động hóa build và triển khai.
- **Cơ Sở Hạ Tầng:** Được lưu trữ trên Azure (VMs, Storage, v.v.).
- **Công Cụ Triển Khai:** Docker và Kubernetes cho container hóa và điều phối.
- **Mô Phỏng Cục Bộ:** Azurite cho các dịch vụ Azure.

## Công Nghệ

| Lớp         | Công Nghệ                    |
|-------------|-----------------------------|
| Frontend    | Vue.js, Vite                |
| Backend     | .NET                        |
| Nhắn Tin    | RabbitMQ                    |
| Cơ Sở Dữ Liệu| PostgreSQL, SQL Server, Redis|
| Xác Thực    | IdentityServer (OIDC)       |
| DevOps      | Jenkins, Docker, Kubernetes |
| Cloud       | Azure                       |

## Thư Viện & Mẫu Thiết Kế Phổ Biến

| Công Cụ/Thư Viện    | Mục Đích                          | Tại Sao Chúng Tôi Sử Dụng Nó        |
|---------------------|-----------------------------------|-------------------------------------|
| `FluentValidation`  | Xác thực đầu vào (C#)             | Logic xác thực sạch, có thể kiểm tra |
| `oidc-client-ts`    | Client OIDC cho Vue.js            | Tích hợp với IdentityServer         |
| `axios`             | Yêu cầu HTTP                      | Nhẹ và được hỗ trợ rộng rãi         |
| `AutoMapper`        | Ánh xạ đối tượng trong C#         | Giảm mã lặp lại                     |
| `MediatR`           | CQRS và kiến trúc sạch trong .NET | Thúc đẩy phân tách mối quan tâm     |

---

## Nguồn Cảm Hứng & Tham Khảo

- [Sairyss/domain-driven-hexagon](https://github.com/Sairyss/domain-driven-hexagon) – Nguồn cảm hứng kiến trúc
- [MichaelCade/90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps) – Cấu trúc chuyển đổi ngôn ngữ
- [Awesome .NET Microservices](https://github.com/thangchung/awesome-dotnet-core#microservices)

## 🗣️ Đóng Góp & Phản Hồi

> Có đề xuất hoặc ý tưởng? Hãy thoải mái mở issue hoặc gửi PR.
