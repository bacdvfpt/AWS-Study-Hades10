# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 1 - Data Storages
1. Data Persistance (Độ bền vững của dữ liệu)
Dựa trên độ bền vững của dữ liệu, có 3 loại data stores dưới đây.
- __Persistent Data Store__ (Lưu trữ dữ liệu lâu dài)
Dữ liệu _durable_ và được lưu trữ cố định, không bị ảnh hưởng khi khởi động lại hoặc tắt server. 
Ví dụ: **Glacier** và **RDS**
- __Transient Data Store__ (Lưu trữ dữ liệu tạm thời)
 Dữ liệu chỉ được lưu trữ tạm thời, sau đó sẽ được chuyển đến một _process_ khác hoặc đến nơi lưu trữ dữ liệu lâu dài - _persistent store_.
 Ví dụ: **SQS** và **SNS**
 - __Ephemeral Data Store__ (Không biết dịch sao nữa ^^)
 Dữ liệu sẽ bị mất khi stop.
 Ví dụ: __EC2 Instance Store__ và __Memcached__
 