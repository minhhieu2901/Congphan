# Phân tích gói tin DHCP bằng wireshark và tcpdump

# Mục lục

### [1. Phân tích gói 4 bản tin DHCP bằng wirshark](https://github.com/phancong0897/Congphan/blob/master/DHCP/DHCP-with-wireshark-and-tcpdump.md#1--ph%C3%A2n-t%C3%ADch-g%C3%B3i-4-b%E1%BA%A3n-tin-dhcp-b%E1%BA%B1ng-wireshark)

### [2. Phân tích gói tin DHCP bằng TCP dump](https://github.com/phancong0897/Congphan/blob/master/DHCP/DHCP-with-wireshark-and-tcpdump.md#2-ph%C3%A2n-t%C3%ADch-g%C3%B3i-tin-dhcp-b%E1%BA%B1ng-tcpdump)

### [ Nguồn tham khảo](https://github.com/phancong0897/Congphan/blob/master/DHCP/DHCP-with-wireshark-and-tcpdump.md#ngu%E1%BB%93n-tham-kh%E1%BA%A3o)

## 1.  Phân tích gói 4 bản tin DHCP bằng wireshark

### Giới thiệu wireshark

- Wireshark, hay còn gọi là Ethereal, công cụ này có lẽ không quá xa lạ với phần lớn người sử dụng chúng ta, vốn được xem là 1 trong những ứng dụng phân tích dữ liệu hệ thống mạng, với khả năng theo dõi, giám sát các gói tin theo thời gian thực, hiển thị chính xác báo cáo cho người dùng qua giao diện khá đơn giản và thân thiện. Trong bài viết dưới đây, chúng tôi sẽ giới thiệu với các bạn một số đặc điểm cơ bản cũng như cách dùng, phân tích và kiểm tra hệ thống mạng bằng Wireshark.

- Sau đây là một ví dụ sử dụng wireshark để bắt gói tin DHCP

<img src="https://imgur.com/Cna5vtR.png">

- DHCP Discovery
    <img src="https://imgur.com/rQP9kRa.png">

    - option (53): Kiểu tin nhắn.
    
    - option (55): Danh sách tham số yêu cầu.

    - User Datagram protocol: thông tin port nguồn , port đích DHCP sử dụng.

- DHCP offer

    <img src="https://imgur.com/x914ykv.png">

    - Internet Protocol v4: chứa địa chỉ IP nguồn, IP đích.

    - option (53): Kiểu tin nhắn.

    - option (54): DHCP server.

    - option (51): thời gian cấp địa chỉ IP.

    - option (1) : subnet mask.

    - option (28): địa chỉ broacast.

    - option (15): Domain Name.

    - option (6) : DNS.

    - option (3) : Deafault gateway.

- DHCP request

    <img src="https://imgur.com/i5sGA0K.png">

    - option (50): địa chỉ IP yêu cầu.

- DHCP ACK

    <img src="https://imgur.com/9RhP3qk.png">

## 2. Phân tích gói tin DHCP bằng TCPDUMP

### Giới thiệu

- TCPDUMP thực chất là công cụ được phát triển nhằm mục đích nhận diện và phân tích các gói dữ liệu mạng theo dòng lệnh. TCPDUMP cho phép khách hàng chặn và hiển thị các gói tin được truyền đi hoặc được nhận trên một mạng có sự tham gia của máy tính.

- Hiểu đơn giản, TCPDUMP là phần mềm bắt gói tin trong mạng làm việc trên hầu hết các phiên bản hệ điều hành unix/linux. Tcpdump cho phép bắt và lưu lại những gói tin bắt được, từ đó chúng ta có thể sử dụng để phân tích.

- TCPDUMP được xem là trụ cột trong việc gỡ rối và kiểm tra vấn đề kết nối mạng và bảo mật.

- Các tùy chọn thường sử dụng trong tcpdump:

    - -X : Hiển thị nội dung của gói theo định dạng ASCII và HEX

    - -XX : Tương tự -X, hiển thị giao diện ethernet

    - -D : Liệt kê các giao diện mạng có sẵn

    - -l : Đầu ra có thể đọc được dòng (để xem khi bạn lưu hoặc gửi đến các lệnh khác)

    - -t : Cung cấp đầu ra dấu thời gian có thể đọc được của con người

    - -q : Ít dài dòng hơn với đầu ra

    - -tttt : Cung cấp đầu ra dấu thời gian tối đa có thể đọc được của con người

    - -i : Bắt lưu lượng của một giao diện cụ thể

    - -vv : Đầu ra cụ thể và chi tiết hơn (nhiều v hơn cho đầu ra nhiều hơn)

    - s : Xác định snaplength(kích thước) của gói tin theo byte. Sử dụng -s0 để có được mọi thứ. Nếu không set size packet dump thành unlimit, thì khi tcpdump ra nó bị phân mảnh

    - -c : Chỉ nhận được x số gói và sau đó dừng lại

    - -S : In số thứ tự tuyệt đối

    - -e : Nhận tiêu đề ethernet

    - -q : Hiển thị ít thông tin giao thức

    - -E : Giải mã lưu lượng IPSEC bằng cách cung cấp khóa mã hóa

### Dùng TCPDUMP bắt gói tin DHCP

- Trước tiên ta cần ccó hai máy centos để test. Một máy cài đặt TCPDUMP, và máy còn lại để xin cấp DHCP.

- Cài đặt TCPDUMP bằng câu lệnh sau:

    - ` yum install tcpdump `

- Sau khi cài đặt xong, chạy lệnh sau để bắt gói tin DHCP

    - ` tcpdump -nni ens33 -n port 67 and port 68 `

<img src="https://imgur.com/lh0hRVE.png">

- Từ hình trên ta có thể thấy Tcpdump bắt được 4 gói tin:

    - Client gửi gói tin DHCP Discover ra toàn mạng, do chưa có địa chỉ IP nên client dùng địa chỉ nguồn 0.0.0.0 ra toàn mạng. Gói tin này có chưa địa chỉ MAC của client.

    - DHCP server nhận được gói tin DHCP Discover, DHCP server sẽ gửi cho client 1 gói tin DHCP offer, gói tin này chưa địa chỉ IP cho thuê một khoảng thời gian nhất định.

    - Client nhận được gói tin DHCP offer, client sẽ gửi trả một gói tin DHCP requet để thông báo cho DHCP server sẽ nhận địa chỉ IP này.

    - DHCP server nhận được gói tin DHCP request, DHCP sẽ gửi gói tin DHCP ACK nhằm thông báo chấp nhận client sử dụng địa chỉ Ip vừa cấp.

    ## Nguồn tham khảo

    https://itforvn.com/tu-hoc-mcsa-mcse-2016-lab-4-cau-hinh-dhcp-server-va-backup-restore/






