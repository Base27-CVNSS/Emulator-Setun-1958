# Trình mô phỏng máy tính tam phân SETUN-1958

**SETUN-1958** là trình mô phỏng bằng ngôn ngữ C của máy tính điện tử số tam phân Setun, còn được gọi là *Máy tính số tự động cỡ nhỏ* (МЦВМ «Сетунь»).

## 1. Thông tin dự án

- Ngày tạo: 01-11-2018
- Ngày cập nhật theo thông tin của dự án gốc: 30-08-2026
- Phiên bản: 2.17
- Tác giả dự án gốc: Vladimir V.
- Liên hệ tác giả gốc: askfind@ya.ru
- Ngôn ngữ triển khai: C
- Giấy phép: GNU GPL phiên bản 3.0

Bản Việt hóa tập trung vào tài liệu và hướng dẫn sử dụng. Tên lệnh, tên tệp, mã nguồn, ký hiệu thanh ghi, định dạng dữ liệu và đầu ra mà chương trình dùng để xử lý máy ảo được giữ nguyên khi cần để không làm thay đổi hành vi của trình mô phỏng.

## 2. Biên dịch và khởi chạy

Mở terminal tại thư mục dự án rồi chạy:

```sh
cd Emulator-Setun-1958
make
```

Lệnh biên dịch tương đương (tùy cấu hình Makefile):

```sh
gcc -v -std=gnu11 -o setun1958emu -g command.c setun1958_vm.c
```

Sau khi biên dịch, khởi chạy chương trình:

```sh
./setun1958emu
```

> Cần có trình biên dịch C hỗ trợ GNU C11 và các thư viện hệ thống mà mã nguồn sử dụng. Nếu biên dịch trên Windows, nên dùng môi trường tương thích POSIX như WSL hoặc MinGW/MSYS2; khả năng tương thích phụ thuộc vào các API hệ thống trong mã nguồn.

## 3. Các lệnh của bảng điều khiển

Tại dấu nhắc của chương trình, nhập `help` để xem danh sách lệnh được phiên bản đang chạy hỗ trợ.

| Lệnh | Bí danh | Chức năng |
|---|---|---|
| `dump` | — | Hiển thị nội dung tệp chương trình hoặc vùng nhớ theo đối số. |
| `load` | `l` | Nạp chương trình từ thư mục nguồn và tạo dữ liệu băng giấy ảo. |
| `prn` | — | Chọn bộ ký tự đầu ra của máy chữ điện. |
| `debug` | `d` | Bật hoặc tắt thông tin gỡ lỗi. |
| `view` | `v` | Hiển thị các thanh ghi và trạng thái máy. |
| `begin` | `b` | Bắt đầu thực thi. |
| `pause` | `p` | Tạm dừng thực thi. |
| `run` | `r` | Tiếp tục thực thi. |
| `step` | `s` | Thực hiện từng bước/lệnh theo cách triển khai của trình mô phỏng. |
| `break` | `br` | Thiết lập điểm dừng theo bộ đếm chương trình (tùy mức hỗ trợ của phiên bản). |
| `breakmb` | `brm` | Thiết lập điểm dừng liên quan đến địa chỉ trống từ/băng trống từ. |
| `reg` | `rg` | Ghi giá trị tam phân vào thanh ghi. |
| `fram` | `fr` | Hiển thị vùng nhớ lõi từ tính FRAM. |
| `drum` | `dr` | Hiển thị vùng bộ nhớ trống từ DRUM. |
| `calc` | — | Gọi chức năng máy tính MK-Setun-01 nếu bản dựng hỗ trợ. |
| `help` | `h` | Hiển thị trợ giúp. |
| `quit` | `q` | Thoát trình mô phỏng. |

Tên lệnh và bí danh ở trên là từ khóa chương trình, **không dịch hoặc đổi tên** khi nhập lệnh.

## 4. Ví dụ sử dụng

### Xem trợ giúp

```text
help
```

### Xem trạng thái các thanh ghi

```text
view
```

Các thanh ghi chính gồm:

- `K`: thanh ghi mã lệnh/địa chỉ theo cách biểu diễn của máy.
- `F`: thanh ghi chỉ số.
- `C`: bộ đếm chương trình.
- `W`: thanh ghi dấu tam phân.
- `ph1`, `ph2`: các trạng thái tràn.
- `S`: bộ tích lũy.
- `R`: thanh ghi nhân.
- `MB`: thanh ghi vùng trống từ.

### Bật và tắt chế độ gỡ lỗi

```text
debug 1
debug 0
```

### Nạp một bộ chương trình mẫu

Ví dụ dưới đây sử dụng đường dẫn tương đối trong kho mã nguồn. Hãy kiểm tra thư mục và tên bộ chương trình có tồn tại trong bản sao của bạn:

```text
load ./software/test2 ./ptr1
```

### Chạy, tạm dừng và tiếp tục

```text
begin
pause
run
```

### Chọn bộ ký tự máy in

```text
prn 0
prn 1
```

Trong bản gốc, hai giá trị này lần lượt chọn bộ ký tự tiếng Nga (`rus`) và chữ Latin (`lat`). Việc đổi ngôn ngữ tài liệu không tự thay đổi bảng mã hoặc hành vi đầu ra của chương trình.

### Xem vùng nhớ và trống từ

```text
fram +
drum 1x
```

Các tham số hợp lệ phụ thuộc vào cú pháp mà trình mô phỏng phiên bản hiện tại hỗ trợ.

### Thoát

```text
quit
```

## 5. Các thiết bị vào/ra ảo

| Tên | Hướng | Mô tả |
|---|---|---|
| `PTR1` | Vào | Bộ đọc băng giấy đục lỗ số 1. |
| `PTR2` | Vào | Bộ đọc băng giấy đục lỗ số 2. |
| `PTP1` | Ra | Bộ đục băng giấy. |
| `TTY1` | Ra | Thiết bị đầu cuối/máy chữ. |
| `DRUM` | Lưu trữ | Bộ nhớ trống từ được mô phỏng. |

Các thư mục `ptr1/`, `ptr2/`, `ptp1/`, `tty1/` và `drum/` chứa dữ liệu của các thiết bị ảo tương ứng trong dự án.

## 6. Hệ tam phân cân bằng

Setun sử dụng hệ tam phân cân bằng: mỗi chữ số tam phân (*trit*) có một trong ba giá trị `-`, `0`, `+`, tương ứng với (-1), (0), (+1).

Một số tam phân cân bằng có giá trị:

`value = Σ(tritᵢ × 3ⁱ)`

Khác với hệ nhị phân dùng hai trạng thái, hệ tam phân cân bằng có ba trạng thái và biểu diễn dấu ngay trong các chữ số. Không được tự ý đổi ký hiệu trong tệp chương trình, ảnh vùng nhớ, băng giấy hoặc dữ liệu trống từ: các ký hiệu đó là một phần của định dạng máy.

## 7. Cấu trúc mã nguồn

- `command.c`, `command.h`: bộ phân tích và điều phối lệnh của giao diện dòng lệnh.
- `setun1958_vm.c`, `setun1958_vm.h`: phần triển khai máy ảo SETUN-1958.
- `software/`: chương trình mẫu và các tài nguyên phần mềm.
- `ptr1/`, `ptr2/`, `ptp1/`, `tty1/`, `drum/`: dữ liệu cho thiết bị vào/ra và bộ nhớ ảo.
- `unittest_setun1958/`: các bài kiểm thử nếu có trong bản sao hiện tại.
- `CHANGELOG.md`: lịch sử thay đổi của dự án.

## 8. Giấy phép và ghi công

Dự án gốc được phân phối theo **GNU General Public License v3.0 (GPL-3.0)**. Khi phân phối hoặc sửa đổi phần mềm, hãy giữ lại thông báo bản quyền, giấy phép và các thông tin ghi công có trong kho mã nguồn. Bản Việt hóa tài liệu không thay thế hoặc sửa đổi các điều khoản của giấy phép.

## 9. Tài liệu tham khảo

- [Tài liệu khoa học máy tính tam phân](http://ternarycomp.cs.msu.su/)
- [Dự án mô phỏng máy Setun](http://trinary.su/projects/)
- [Bài viết về mã tam phân của Setun trên Habr](https://habr.com/ru/company/ua-hosting/blog/273929/)
- [Cộng đồng Setun trên VK](https://vk.com/setunsu/)
- [Cộng đồng Setun trên Telegram](https://t.me/setun_1958/)

## 10. Đóng góp

Hoan nghênh mọi đóng góp giúp cải thiện tài liệu, ví dụ, kiểm thử và khả năng sử dụng của trình mô phỏng. Vui lòng giữ nguyên các định dạng dữ liệu và giao diện lệnh hiện có, đồng thời mô tả rõ thay đổi nếu sửa hành vi của máy ảo.
