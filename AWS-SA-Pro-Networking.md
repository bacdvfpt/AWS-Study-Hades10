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
__Use cases__
- Kết nối AWS managed IPsec VPN đến một VPC thông qua Internet.
__Lợi thế__
- Sử dụng lại processes và VPN có sẵn.
- Sử dụng lại kết nối internet có sẵn.
- Tiếp cận cách kết nối này khi muốn thực hiện connect từ remote networks vào VPC một cách an toàn, redundancy và failover.
- Virtual Private Gateway supports kết nối với nhiều User gateways -> Có thể triển khai redundancy và failover ở phía remote networks (VPN Connection).
- Hỗ trợ static routes, dynamic routes(BGP peering).
__Hạn chế__
- Phụ thuộc vào Internet.
- Tự chịu trách nhiện về redundancy và failover ở phía customer endpoint nếu yêu cầu.
- Customer device phải support single-hop BGP nếu như sử dụng BGP cho dynamic routing.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsmanagedvpn.png" alt="AWS Managed VPN">
</p>

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/redundantawsmanagedvpn.png" alt="Redundant AWS Managed VPN Connections">
 </p>

##### Cách thực hiện
- Phía AWS:
    - 1. Tạo Virtual Private Gateway(VPG) và attach vào VPC.
    - 2. Tạo Customer Gateway(CGW) trên AWS.
    - 3. Tạo VPN Connection, link VPG và VGW.
    - 4. Download CGW config từ aws.
- Phía Customer:
    - 1. Config gateway kết nối đến AWS VPN Connection.
    - 2. (Optional) Config BGP Routing.

##### Khái niệm
* __VPN connection__: A secure connection between your on-premises equipment and your VPCs.
* __VPN tunnel__: An encrypted link where data can pass from the customer network to or from AWS.
* __Customer gateway__: An AWS resource which provides information to AWS about your customer gateway device.
* __Customer gateway device__: A physical device or software application on your side of the Site-to-Site VPN connection.

#### 2. AWS Direct Connect
__Use cases__
- Cung cấp kết nối network chuyên dụng thông qua private lines từ on-premises network đến một hoặc nhiều VPCs trong cùng một region.
__Lợi thế__
- Dễ kiểm soát hiệu suất network. (Vì chỉ mình sử dụng)
- Giảm bandwidth costs.
- Hỗ trợ BGP peering và routing policies.
__Hạn chế__
- Thêm nhiều setting hơn.

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsdirectconnect.png" alt="AWS Direct Connect">
 </p>

Multiple dynamically routed AWS Direct Connect connections (Nâng cao tính HA - tính sẵn sàng)

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/redundantawsdirectconnect.png" alt="Redundant AWS Direct Connect">
 </p>

__Direct Connect gateway__ - global resource \
Cho phép kết nối nhiều VPCs khác region hoặc khác AWS account.

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsdirectconnectgateway.png" alt="AWS Direct Connect Gateway">
 </p>

#### 3. AWS Direct Connect + VPN
__Use cases__
Kết hợp AWS Direct Connect dedicated network connections và Amazon VPC VPN. Cung cấp kết nối IPsec VPN bảo thông qua đường truyền riêng. \
Sự kết hợp này giúp giảm độ trễ (low latency) của IPsec Connection và tăng bandwidth của AWS Direct Connect -> Tốt hơn internet-based VPN Connections. \ 
Dùng khi muốn dùng encrypted tunnel thông qua Direct Connect. \
__Lợi thế__
- Dễ kiểm soát hiệu suất network. (Vì chỉ mình sử dụng)
- Giảm bandwidth costs.
- Hỗ trợ BGP peering và routing policies trên AWS DX.
- Sử dụng lại processes và VPN có sẵn.
- Hỗ trợ static routes, dynamic routes(BGP peering).
__Hạn chế__
 phải setting nhiều hơn, tốn tiền hơn.

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsdirectconnectvpn.png" alt="AWS Direct Connect + VPN">
 </p>

#### 4. AWS VPN CloudHub
 Xây dựng dựa trên AWS managed VPN, có thể giao tiếp với các bên khác một cách bảo mật thông qua AWS VPN CloudHub. \
 AWS VPN CloudHub vận hành trên model hub-and-spoke -> có thể dùng với AWS VPC hoặc không. \
 Dùng khi có nhiều branch muốn kết nối với nhau một cách bảo mật, thuận tiện, sử dụng kết nối internet có sẵn. Link các remote offices(branches) với nhau để backup hoặc access thông nhau.
__Use cases__
 - Kết nối remote branch offices với nhau thông qua hub-and-spoke model (Hub là trung gian cho các spoke).
__Lợi thế__
- Sử dụng lại kết nối internet và AWS VPN.
__Hạn chế__
- Phụ thuộc vào Internet
- Tự chịu trách nhiệm về redundancy và failover.

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsvpncloudhub.png" alt="AWS VPN CloudHub">
 </p>

#### 5. Software Site-to-Site VPN
 Full managed cả hai phía của Amazon VPC connectivity bằng cách tạo VPN connection giữa remote network và một software VPN được setting ở trong VPC.
 Dùng khi phải quản lý cả 2 đầu của VPN Connection, hoặc phải setting những thứ mà Amazon VPC’s VPN chưa support.
__Use cases__
- Software appliance-based VPN connection thông qua internet.
__Lợi thế__
- Support nhiều bên cung cấp VPN, nhiều sản phẩm VPN và giao thức hơn.
- Fully Managed.
__Hạn chế__
- Customer chịu trách nhiệm hết tất cả mọi thứ.

 <p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/softwarevpn.png" alt="Software VPN">
 </p>

#### 6. Transit VPC
 Được thay thế bởi AWS Transit Gateway

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/Transitvpc.png" alt="Transit VPC">
</p>

#### 7. AWS Transit Gateway + VPN
Transit Gateway -> Regional \
IPsec VPN connection giữa remote network và Transit Gateway \
Kết nối thông qua internet
__User cases__
- Kết nối AWS managed IPsec VPN đến regional router cho nhiều VPC thông qua Internet.
__Lợi thế__
- Giống AWS Managed VPN.
- Có thể dùng cho nhiều VPC trên một region.
__Hạn chế__
- Giống AWS Managed VPN.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/transitgwvpn.png" alt="Transit Gateway + VPN">
</p>

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/transitgwredundantvpn.png" alt="Transit Gateway + Redundant VPN">
</p>

#### 8. AWS Direct Connect + AWS Transit Gateway
Tối đa 3 regions. \
__Use cases__
- Cung cấp kết nối network chuyên dụng thông qua private lines từ on-premises network đến nhiều VPCs cho nhiều regions.
__Lợi thế__
- Dễ kiểm soát hiệu suất network. (Vì chỉ mình sử dụng)
- Giảm bandwidth costs.
- Hỗ trợ BGP peering và routing policies.
- Tính HA và tính scalable.
__Hạn chế__
- Thêm nhiều setting hơn.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/transitgwdirectconnect.png" alt="AWS Direct Connect + AWS Transit Gateway">
</p>

#### 9. AWS Direct Connect + AWS Transit Gateway + VPN
Regional. \
__Use cases__
- Cung cấp kết nối AWS managed IPsec thông qua private lines từ on-premises network đến một hoặc nhiều VPCs trong cùng một region.
__Lợi thế__
- Dễ kiểm soát hiệu suất network. (Vì chỉ mình sử dụng)
- Giảm bandwidth costs.
- Hỗ trợ BGP peering và routing policies trên AWS DX.
- Sử dụng lại processes và VPN có sẵn.
- Hỗ trợ static routes, dynamic routes(BGP peering).z
- Tinh HA và tính scalable.
__Hạn chế__
- Thêm nhiều setting hơn.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/transitgwdxvpn.png" alt="AWS Direct Connect + AWS Transit Gateway + VPN">
</p>

### Networking - VPC to VPC Connectivity

#### 1. VPC peering
__Use cases__
- Kêt nối 2 VPCs với nhau thông qua network được AWS cung cấp.
__Lợi thế__
- Tận dụng được kiến trúc network của AWS rất scalable.
__Hạn chế__
- Không support kết nối bắc cầu các VPCs.
- Khó quản lý khi mà số lượng VPCs tham gia vào mạng lưới nhiều.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/vpcpeering.png" alt="VPC Peering">
</p>

#### 2. AWS Transit Gateway
__Use cases__
- Kêt nối các VPCs trong cùng region với nhau thông qua router được AWS cung cấp.
__Lợi thế__
- HA và scalable.
__Hạn chế__
- Transit Gateway peering chỉ thực hiện peering across regions chứ không phải trong nội bộ region.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/transitgateway.png" alt="AWS Transit Gateway">
</p>

#### 3. Software Site-to-Site VPN
__Use cases__
- Sử dụng Software appliance-based VPN kết nối giữa các VPCs
__Lợi thế__
- Support nhiều VPN vendors, product, protocols.
- Hoàn toàn được quản lý bởi user.
__Hạn chế__
- User chịu trách nhiệm hết về HA cũng như khả năng scale.
- Sử dụng VPN instance sẽ dẫn đến bottleneck.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/softwaresitetositevpn.png" alt="Software Site-to-Site VPN">
</p>

#### 4. Software VPN-to-AWS Managed VPN
__Use cases__
- Software appliance to VPN connection between VPCs
__Lợi thế__
- AWS quản lý về tính HA của kết nối VPC VPN.
- Support nhiều VPN Vendors, product được quản lý bởi user.
- Hỗ trợ static routes, dynamic routes(BGP peering) và routing policies.
__Hạn chế__
- User chịu trách nhiệm hết về HA cũng như khả năng scale.
- Sử dụng VPN instance sẽ dẫn đến bottleneck.
- Chỉ sử dụng được IPSec VPN protocol cho AWS Managed VPN.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/softwarevpntoawsmanagedvpn.png" alt="Software VPN-to-AWS Managed VPN">
</p>

#### 5. AWS Managed VPN
__Use cases__
- Định tuyến VPC-to-VPC quản lý bởi user thông qua IPsec VPN connections sử dụng các phương thức của user.
__Lợi thế__
- AWS quản lý về tính HA của kết nối VPC VPN.
- Hỗ trợ static routes, dynamic routes(BGP peering) và routing policies.
__Hạn chế__
- Khi sử dụng các phương thức của user quản lý thì user tự chịu trách nhiệm về redundancy and failover của enpoint đó.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsmanagedvpnvpctovpc.png" alt="AWS Managed VPN VPC-to-VPC">
</p>

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsdxvpctovpc.png" alt="AWS Direct Gateway VPC-to-VPC Routing">
</p>

#### 6. AWS PrivateLink
__Use cases__
- Kêt nối 2 VPCs với nhau thông qua network được AWS cung cấp sử dụng interface endpoints.
__Lợi thế__
- Tận dụng được kiến trúc network của AWS rất scalable.
__Hạn chế__
- VPC Endpoint services chỉ hoạt động trong phạm vi region mà nó được tạo(trừ khi các VPC được connect với nhau qua VPC peering).

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/networking/awsprivatelink.png" alt="AWS PrivateLink">
</p>

__Interface Endpoint__
- Private IP ENI.
- Dùng DNS entries để điều hướng traffic.
- API Gateway, CF, CW, etc.
- Bảo mật: Security groups.

__Gateway Endpoint__
- target của một route cụ thể.
- Dùng prefix lists trong route table để điều hướng traffic.
- Chỉ có 2 services hỗ trợ: S3 và DynamoDB
- Bảo mật: dùng VPC Endpoint policies.

### Networking - Internet Gateway

#### 1. Internet Gateway

[Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)

#### 2. Egress-only Internet Gateways

- enable outbound-only
- IPv6 traffic only

[Egress-only Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/egress-only-internet-gateway.html)

#### 3. NAT instances
- enable outbound-only
- IPv4 traffic only
- Phải nằm trong public subnet.
- User chịu trách nhiệm về NAT instance(có thể bị bottleneck).
- Có thể dùng SG.
- Có thể dettach EIP.
- Có thể dùng bastion server.

[NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)

#### 4. NAT gateways
- enable outbound-only
- IPv4 traffic only
- Phải nằm trong public subnet.
- AWS Managed service.
- Thuộc về AZ.
- Không thể dùng để access VPC peering, VPN và DX.
- Không thể dùng SG.
- Không thể dettach EIP.
- Không thể dùng bastion server.
[NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)

### Networking - Routing

#### 1. Route Tables
- he most specific route that matches the traffic (longest prefix match).

#### 2. BGP - Border Gateway Protocol
????

### Networking - Enhanced Networking

#### 1. Placement groups
__Cluster__
- các instances nằm trong 1 AZ.
- low-latency network performance

__Partition__
- spread các instances vào logical partitions.
- group instances trong một partition không thể share hardware với group instances trong một partition khác.
- dùng cho hệ thống lớn, workload nặng.

__Spread__
- Phân bố đều.
- Nếu hardware có vấn đề thì không chết hết.
- Có thể phân bố multi AZ.
- Tối đa 7 instances / group / AZ. 

### Networking - Route 53