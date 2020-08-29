# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 1 - Data Storages
### Data Stores - Concepts
#### 1. Data Persistance (Độ bền vững của dữ liệu) \
Dựa trên độ bền vững của dữ liệu, có 3 loại data stores dưới đây.
- __Persistent Data Store__ (_Lưu trữ dữ liệu lâu dài_) \
Dữ liệu _durable_ và được lưu trữ cố định, không bị ảnh hưởng khi khởi động lại hoặc tắt server. \
Ví dụ: **Glacier** và **RDS**
- __Transient Data Store__ (_Lưu trữ dữ liệu tạm thời_) \
 Dữ liệu chỉ được lưu trữ tạm thời, sau đó sẽ được chuyển đến một _process_ khác hoặc đến nơi lưu trữ dữ liệu lâu dài - _persistent store_. \
 Ví dụ: **SQS** và **SNS**
 - __Ephemeral Data Store__ (_Không biết dịch sao nữa ^^_) \
 Dữ liệu sẽ bị mất khi stop. \
 Ví dụ: __EC2 Instance Store__ và __Memcached__
 #### 2. IOPS vs. Throughput
 - __IOPS - Input/Output Operations per Second__ \
 Thể hiện tốc độ đọc và ghi. Phù hợp với những hệ thống yêu cầu cao về tốc độ đọc ghi. \
 - __Throughput__ \
 Thể hiện lượng dữ liệu có thể đi qua (_Through_) trong một khoảng thời gian \
 #### 3. Consistency (Tính nhất quán)
 - __Consistency Models__ \
 Có 2 

