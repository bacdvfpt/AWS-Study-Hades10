# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 1 - Data Storages

### Data Stores - Concepts

#### 1. Data Persistance (Độ bền vững của dữ liệu) 
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
 Thể hiện tốc độ đọc và ghi. Phù hợp với những hệ thống yêu cầu cao về tốc độ đọc ghi. 
- __Throughput__ \
 Thể hiện lượng dữ liệu có thể đi qua (_Through_) trong một khoảng thời gian.

#### 3. Consistency (Tính nhất quán)
- __Consistency Models__
 - __ACID__: Tập hợp các property của database transactions để đảm bảo dữ liệu _valid_ dù cho có gặp sự cố(gặp lỗi, mất điện, etc).
  - __Atomicity__: Transactions là "_Tất cả hoặc không có gì_". Nghĩa là một transactions được coi là thành công nếu tất cả mọi thao tác phải thành công, nếu một thao tác không thành công thì transaction đó sẽ thất bại. 
  - __Consistency__: Transactions phải luôn hợp lệ. Nghĩa là một transaction chỉ có thể đưa database từ trạng thái hợp lệ này sang trạng thái hợp lệ khác để đảm bảo tính bất biến của dữ liệu.
  - __Isolation__: Đảm bảo tính cô lập của dữ liệu. Transaction này không thể lẫn lộn hay ảnh hưởng đến transactions khác. Transactions dù được thực thi đồng thờI cũng phải được đảm bảo tính cô lập, không ảnh hưởng đến cái khác. 
  - __Durablity__: Transaction đã được hoàn thành phải đưỢc lưu trữ lâu dài, cho dù có sự cố xảy ra thì vẫn đảm bảo sau khi khôi phục hệ thống vẫn dùng được dữ liệu.
 - __BASE__: Ngược lại với ACID.
  - __Basically Available__: Đảm bảo tính khả dụng. Dữ liệu luôn khả dụng ngay cả khi dữ liệu đó cũ. ??
  - __Soft State__: trạng thái của dữ liệu, cơ sở dữ liệu không nhất thiết phải nhất quán mọi lúc. Nó có thể thay đổi.
  - __Eventual Consistency__: Dữ liệu sẽ đạt được trạng thái nhất quán ở một thời điển nào đó.
 Giải thích __BASE__(đúng hơn là __Eventual Consistency__): Nếu như một record trong một database không có thay đổi trong một khoảng thời gian,thì trong khoảng thời gian đó nếu thực hiện _request(read data)_ tất cả các client sẽ nhận về cùng một dữ liệu của record đó. Nếu có thực hiện update record, tất cả các replica của database sẽ được thực hiện đồng bộ data, ở một thời điển nào đó client A sẽ nhận được record có nội dung trước khi update và client B sẽ nhận được record có nội dung sau khi update. Nhưng đến một thời điển nào đó, dữ liệu sẽ đạt được tính nhất quán và tất cả các client sẽ nhận được record có nội dung sau khi đã update.

#### 4. Amazon S3
- S3 là Object Store. Có thể lưu trữ tối đa 5GB / object, nhưng size tốt đa của object trong 1 _PUT_ là 5GB. /
-> Phải dùng multi-part uploads nếu như object lớn hơn 5GB. Recommend: Object > 100Mb là dùng multi-part uploads.
- S3 không có file path, thay vào đó mỗi object sẽ có 1 unique KEY.

