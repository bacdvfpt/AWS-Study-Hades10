# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 2 - Networking

### Networking - Concepts

#### 1. OSI Model
OSI Model có 7 Layer(Từ thấp đến cao)
- __Physical__ (_Tầng vật lý_)  - AWS chịu trách nhiệm
- __Data Link__ (_Tầng liên kết_) - AWS chịu trách nhiệm
- __Network__ (_Tầng mạng_) - Customer chịu trách nhiệm
- __Transport__ (_Tầng giao vận_) - Customer chịu trách nhiệm
- __Session__ (_Tầng phiên_) - Customer chịu trách nhiệm
- __Presentation__ (_Tầng trình bày/ trình diễn_) - Customer chịu trách nhiệm
- __Application__ (_Tầng ứng dụng_) - Customer chịu trách nhiệm

#### 2. TCP vs UDP vs ICMP
- __TCP__ 
    - Hướng kết nối: phải thiết lập kết nối trước khi thực hiện truyền dữ liệu.
    - Stateful
    - Đảm bảo chất lượng gói tin. Vì có cơ chế đánh số thứ tự gói tin và cơ chế báo nhận(đã nhận được gói tin thì báo lại bên gửi)
    - Mất thời gian hơn UDP
    - Ví dụ: Web, Email, File Transfer
- __UDP__ 
    - Hướng phi kết nối: không cần thiết lập kết nối trước khi gửi gói tin.
    - Stateless
    - Không quan tâm chất lượng gói tin, chỉ gửi gói tin và không quan tâm đến việc nhận gói tin có chính xác không? có đúng địa chỉ nhận không.
    - Phù hợp với các ứng dụng yêu cầu truyền tải nhanh, liên tục và không bị ảnh hưởng khi mất một vài gói tin.
    - ví dụ: Streaming media, DNS
- __ICMP__ 
    - Giao thức điều khiển truyền tin trên mạng.
    - ICMP được dùng để thông báo các lỗi xảy ra trong quá trình truyền đi của các gói dữ liệu trên mạng. Hay dùng để thăm dò và quản lý quá trình hoạt động của mạng.
    - ICMP không phải là giao thức truyền tải gửi dữ liệu giữa các hệ thống với nhau mà chúng được xem như bộ định tuyến (Các thiết bị network trao đổi thông tin với nhau).
    - ví dụ: traceroute, ping

### Networking - Network to VPC Connectivity

#### 1. AWS Managed VPN
Amazon VPC cung cấp một option cho phép tạo một kết nối IPsec VPN giữa remote networks và Amazon VPC thông qua internet.

![awsmanagedvpn](resources/images/networking/awsmanagedvpn.png?style=centerme)


