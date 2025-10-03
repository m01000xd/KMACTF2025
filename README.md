# KMACTF2025
### simple
<img width="729" height="537" alt="image" src="https://github.com/user-attachments/assets/a3c901eb-ab35-4654-b351-5eb22645ffba" />

1 file PE64, không bị pack

Chạy thì chương trình yêu cầu 2 tham số, nếu chỉ có file thực thi thì chương trình 

<img width="555" height="106" alt="image" src="https://github.com/user-attachments/assets/b9bd078b-8a04-40ae-b3b6-77c384288207" />

Thử nhập 1 chuỗi linh tinh

<img width="710" height="67" alt="image" src="https://github.com/user-attachments/assets/cd3394eb-de79-49a3-ab86-e771d600e79e" />

Cho vào IDA để phân tích thử

<img width="661" height="558" alt="image" src="https://github.com/user-attachments/assets/623ef125-32f0-4cd7-836f-d14aed1acda2" />

Đặt breakpoint đầu hàm main

<img width="409" height="240" alt="image" src="https://github.com/user-attachments/assets/78849c4a-c470-404f-a3ba-40befaa2498f" />

Thử F8 đến hết hàm main thì khi chạy qua hàm sub_7FF622A36990

<img width="409" height="240" alt="image" src="https://github.com/user-attachments/assets/b8208bbf-a4c1-4b1f-b0f1-9e8e38ce1915" />

<img width="974" height="275" alt="image" src="https://github.com/user-attachments/assets/442511d7-22c8-461e-b11a-66c3c928a23d" />

Command line hiện thông báo như khi ta chạy chay

Vậy ta đoán phần xử lí thông báo kia được gọi trong hàm này, ta sẽ F7 vào để xem nó làm gì

<img width="469" height="350" alt="image" src="https://github.com/user-attachments/assets/08ff10e7-f9f7-4411-a236-ad4e9f17a0dc" />

Ta cứ F8 như lúc nãy để xem khi chạy qua hàm nào thì cmd sẽ trả về thông báo như kia

<img width="432" height="245" alt="image" src="https://github.com/user-attachments/assets/9282e39e-6063-444f-9f23-95a0e2de91bf" />

F8 thì khi chạy qua hàm sub_7FF622A26C40

<img width="974" height="275" alt="image" src="https://github.com/user-attachments/assets/442511d7-22c8-461e-b11a-66c3c928a23d" />

Cmd lại hiện thông báo như lúc nãy

Ta lại F7 vào hàm này

<img width="808" height="361" alt="image" src="https://github.com/user-attachments/assets/310c44d7-f231-40dc-bb88-8893e33ee579" />

<img width="777" height="377" alt="image" src="https://github.com/user-attachments/assets/564147dd-99a0-4af5-a907-66ea660173d9" />

<img width="724" height="374" alt="image" src="https://github.com/user-attachments/assets/77a95bad-ba48-4760-924e-c0e83a4e3211" />

Hàm này gọi đến khá nhiều hàm phụ. Ta biết rằng hiện thông báo Usage file <Flag> là vì ta mới chỉ truyền 1 tham số, còn nếu hiện là Incorrect thì phải là 2 tham số, mà khi gọi qua hàm này cmd lại hiện thông báo như vậy, vậy phải có các đoạn if else nào đó để kiểm tra tham số truyền vào và in ra console thông báo

<img width="569" height="70" alt="image" src="https://github.com/user-attachments/assets/e620918b-e7d2-4c46-acfd-5eac252c24ec" />

Ta để ý thấy có 1 đoạn if này đang kiểm tra cái gì đó và return giá trị của hàm sub_7FF6229E71B0

Thử kiểm tra xem tham số unk_7FF622A52FC0 là gì


<img width="552" height="361" alt="image" src="https://github.com/user-attachments/assets/21cafa86-4b92-4f43-a44a-8158dfed116a" />

<img width="292" height="302" alt="image" src="https://github.com/user-attachments/assets/8c2e360b-7fc8-442d-882d-465567123b8b" />

<img width="232" height="184" alt="image" src="https://github.com/user-attachments/assets/a1707f22-4de7-4d70-894e-d98ca6123a81" />

Ta thấy tham số truyền vào chứa chuỗi Usage: <Flag> dạng wide, vậy hàm lúc nãy truyền chuỗi nãy vào chính là hàm in ra console

Trace ngược lại phần if

<img width="572" height="109" alt="image" src="https://github.com/user-attachments/assets/88a58389-d59a-48bd-bde4-d2059c131ecf" />

Ta thấy nó kiểm tra giá trị của con trỏ tại địa chỉ v2 + 8 nếu nó khác 0 sẽ in ra cmd Usage <Flag>

<img width="203" height="31" alt="image" src="https://github.com/user-attachments/assets/2c8d907e-3ec6-4663-828e-1bb64b1ede4d" />

v2 được lấy từ kết quả return từ hàm sub_7FF6229EB800

<img width="444" height="262" alt="image" src="https://github.com/user-attachments/assets/dfe51aa5-7483-438b-9285-0d62438a1d46" />

Hàm này lấy commandline hiện tại của file thực thi, vâỵ nó sẽ check command line hiện tại để in ra command thông báo như trên

Ta để ý phần hiện thông báo Usage đến từ 1 nhánh else

<img width="626" height="132" alt="image" src="https://github.com/user-attachments/assets/223d238d-fabd-4e3e-81a9-2c930d3a96c2" />

Ta sẽ trace ngược lại nhánh if xem nó kiểm tra cái gì

<img width="723" height="350" alt="image" src="https://github.com/user-attachments/assets/e9fd7a9b-56af-4ee7-8801-ffbc26b50e05" />

Nhánh if sẽ kiểm tra giá trị DWORD của 1 con trỏ tại địa chỉ a1+8, nếu nó = 1 thì sẽ gọi đến 1 loạt các hàm phụ để xử lý hay tính toán cái gì đó, còn nếu # 1 thì sẽ đến nhánh hiện thông báo Usage. Có vẻ như nhánh if đang kiểm tra tham số truyền vào.

<img width="448" height="70" alt="image" src="https://github.com/user-attachments/assets/a89432b5-bf5c-4c62-bbc5-e47987d694f9" />

Ta thấy ở cuối nhánh if có 1 đoạn kiểm tra giá trị v14 với 0, và tùy vào đó nó sẽ return cùng 1 hàm sub_7FF6229E7190 nhưng khác tham số truyền vào 

Kiểm tra tham số đầu là gì

<img width="535" height="251" alt="image" src="https://github.com/user-attachments/assets/6beb661c-0fc5-4b9b-9661-6e0dd6982ae9" />

<img width="439" height="250" alt="image" src="https://github.com/user-attachments/assets/b7bcfe9e-32f7-4c6a-ba53-818d7b566249" />

Tham số này là địa chỉ của chuỗi Correct theo dạng wide

<img width="442" height="320" alt="image" src="https://github.com/user-attachments/assets/0a5ac306-efbd-44ab-b837-7ca889f64742" />

Còn tham số kia là địa chỉ của chuỗi Incorrect

Vậy nếu v14 khác 0 thì chương trình sẽ in ra console là Correct, còn nếu = 0 thì là Incorrect

<img width="723" height="109" alt="image" src="https://github.com/user-attachments/assets/924891e6-9595-42cf-9e8b-81486273243a" />

Ta thấy lúc đầu v14=1, sau đó chương trình kiểm tra lần lượt 80 kí tự của 2 mảng v10 và v4, nếu 2 mảng không giống nhau thì v14 sẽ = 0, tức in ra Incorrect

Thử xref thì v4 được lấy từ hàm sub_7FF622981F00

<img width="699" height="220" alt="image" src="https://github.com/user-attachments/assets/dc3573f2-8b34-495f-a69e-6204695bf479" />

<img width="845" height="317" alt="image" src="https://github.com/user-attachments/assets/4a683990-f0fe-421e-8702-176c771ffcd3" />

Hàm này là 1 TLS allocator, nó truyền vào a1 là 1 con trỏ tới 1 struct, và a2 là số lượng phần tử cần cấp phát
Nếu a2 > 0x7FFFFFFF(2^31-1), thì nó sẽ gọi hàm sub_7FF622A10FC0 để xử lý exception

<img width="852" height="19" alt="image" src="https://github.com/user-attachments/assets/9a31f587-ca92-41c1-879a-f0948c9be255" />

Nó lấy một con trỏ tới vùng TLS (Thread Local Storage) → tức allocator này quản lý bộ nhớ riêng cho từng thread

Phần sau để xử lý tràn

<img width="684" height="626" alt="image" src="https://github.com/user-attachments/assets/e92786a0-2c7d-458a-86d3-76d0b355f888" />

Hàm sub_7FF622981F00 được gọi rất nhiều nên ta sẽ rename lại để sau gặp lại thì biết nó làm gì

<img width="336" height="23" alt="image" src="https://github.com/user-attachments/assets/5b746071-f267-4659-8108-f2639c3fd335" />

Vậy v4 được cấp 1 vùng nhớ có kích thước 80 byte

Sau đó v4 được truyền vào hàm sub_7FF6229E4D50

<img width="488" height="363" alt="image" src="https://github.com/user-attachments/assets/5ed6af5a-1750-4970-ad54-723b9f71c648" />

Hàm này là 1 hàm memcpy nhưng được tối ưu bằng cách sử dụng SIMD. Ta cũng sẽ rename lại hàm 

<img width="332" height="24" alt="image" src="https://github.com/user-attachments/assets/a5fbbbde-1491-4485-a41b-0a9bb428092a" />

Vậy là hàm trên sẽ copy 80 byte từ địa chỉ unk_7FF622A9A980 vào vùng v4+16

Vậy v4 sẽ chứa 80 byte tại địc chỉ unk_7FF622A9A980, giờ ta sẽ trace ngược v10

<img width="698" height="234" alt="image" src="https://github.com/user-attachments/assets/d025ffca-ada6-4e0f-bfcc-cc154b31fb11" />

v10 được cấp phát 1 vùng nhớ tại địa chỉ có kích thước v9

Ta trace ngược lại xem v9 được lấy từ đâu

<img width="231" height="22" alt="image" src="https://github.com/user-attachments/assets/752b3752-f22c-4c99-a160-933efcafdfe7" />

v9 được lấy từ con trỏ tại địa chỉ v7+8

<img width="285" height="27" alt="image" src="https://github.com/user-attachments/assets/c2389951-c7ef-4960-807f-e687a8400520" />

v7 được lấy từ hàm sub_7FF622A26BE0

<img width="497" height="286" alt="image" src="https://github.com/user-attachments/assets/b698d5ab-55b8-4b54-a34c-0f6d194bb7e0" />


Hàm này chính xác là PKCS#7 padding, nó truyền vào là 1 con trỏ tới 1 struct, và 1 số nguyên a2

<img width="348" height="49" alt="image" src="https://github.com/user-attachments/assets/b1204595-4498-4cbd-b09e-cc8f563e39fa" />


v3 lấy chiều dài (hoặc size) từ offset +8 trong struct a1

<img width="224" height="22" alt="image" src="https://github.com/user-attachments/assets/67319028-a4af-41f5-a66f-69ef2f69e02b" />

v4 = số byte cần thêm để làm cho v3 chia hết cho a2

<img width="279" height="23" alt="image" src="https://github.com/user-attachments/assets/b1b8b398-d898-40f9-ae5f-28ce30feb87b" />

Ta để ý a2 = 16, vậy đây chính là mã hóa AES

<img width="340" height="22" alt="image" src="https://github.com/user-attachments/assets/8280a183-38c8-4ff4-ae63-c20a0b9157f8" />

v5 được cấp một vùng nhớ mới có kích thước = dữ liệu cũ + padding.

<img width="297" height="25" alt="image" src="https://github.com/user-attachments/assets/25f662e5-783a-4c5a-b16f-e358f596a6e7" />

Sau đó nó gọi hàm sub_7FF6229EA640

<img width="764" height="380" alt="image" src="https://github.com/user-attachments/assets/851c188f-2d4f-4539-8613-57d078c92f03" />

<img width="587" height="384" alt="image" src="https://github.com/user-attachments/assets/a9eabb54-f11d-44b9-ad52-a0872c82a902" />

<img width="643" height="373" alt="image" src="https://github.com/user-attachments/assets/8cf664ee-dc06-4a8a-a208-d2a3cb285d30" />

Hàm này là 1 hàm copy dữ liệu an toàn từ buffer A sang buffer B với kiểm tra giới hạn.

<img width="430" height="114" alt="image" src="https://github.com/user-attachments/assets/2171f217-9546-42e8-a10d-72b5729da4d2" />

Rename lại. Vậy hàm trên sẽ copy v3 byte từ buffer gốc (v2) sang buffer mới (v5). Sau khi copy dữ liệu gốc, vòng for sẽ điền thêm v4 byte padding vào cuối buffer. Giá trị mỗi byte padding = v4.

Vậy ta sẽ rename lại hàm PKCS#7 này

<img width="418" height="302" alt="image" src="https://github.com/user-attachments/assets/031af5fa-a752-475e-ad76-971b48851832" />

Vậy ta biết được v10 chính là input nhập vào và qua hàm padding để độ dài input luôn chia hết cho 16. v10 sau khi được mã hóa AES sẽ được check với v4 là cipher có độ dài 80 byte chính là đây

<img width="689" height="27" alt="image" src="https://github.com/user-attachments/assets/e718e905-b5ca-4d02-8a13-c437d1450b97" />


<img width="631" height="313" alt="image" src="https://github.com/user-attachments/assets/355ba531-eec4-4a95-8eba-5e23a9d7650e" />

Ta tiếp tục trace đến đoạn nào gọi đến v10

<img width="523" height="111" alt="image" src="https://github.com/user-attachments/assets/4b687c65-7c4e-4d6a-9e6b-48648743970c" />

Nó được duyệt (v9//16 -1) lần trong 1 vòng for và được gọi bởi hàm safe_memcpy lúc nãy ta đã rename.

<img width="275" height="27" alt="image" src="https://github.com/user-attachments/assets/6006e4c2-172a-4d29-9b09-5164a3c91630" />

Nó được copy 16 byte từ v13

<img width="273" height="25" alt="image" src="https://github.com/user-attachments/assets/47554168-60a1-472a-8e47-8460154cd4fd" />

v13 thì được gọi từ hàm sub_7FF622A27A00 

<img width="635" height="184" alt="image" src="https://github.com/user-attachments/assets/ae682760-1b07-49bf-aaf4-a6478be087c6" />

Hàm này có vẻ là phần AES encryption vì

<img width="635" height="184" alt="image" src="https://github.com/user-attachments/assets/cc87ed84-ee2b-4bd9-84b7-ae631834e0e3" />

Phần đầu nó copy input block (16 byte) vào state

<img width="400" height="171" alt="image" src="https://github.com/user-attachments/assets/5a3d14d9-5ce0-4ddb-a8d2-b71cef7f558c" />

Ở dưới nó gọi 10 lần, tức 10 round, 4 step trong encrypt AES, SubBytes, ShiftRows, MixColumns, AddRoundKey. Ta có thể vào các hàm đó để check xem

<img width="674" height="392" alt="image" src="https://github.com/user-attachments/assets/f9ce3120-2185-450a-8f7d-31854962816d" />

<img width="926" height="304" alt="image" src="https://github.com/user-attachments/assets/c09eccd2-bff4-4e5f-867e-44c6e6dd7a76" />

<img width="923" height="356" alt="image" src="https://github.com/user-attachments/assets/fd339e30-2284-41ef-be6a-c441b9f51e08" />

<img width="864" height="387" alt="image" src="https://github.com/user-attachments/assets/18b8c576-c713-40ef-a378-c7dc73a05078" />

Ví dụ như hàm đầu trong vòng for ta có thể khẳng định đây là hàm SubBytes, bởi vì khi check hàm này:

<img width="545" height="180" alt="image" src="https://github.com/user-attachments/assets/3b2c51b3-d995-410c-b191-6006cedc64c1" />

Phần này nó lặp qua 4 cột (mỗi cột 4 byte → tổng 16 byte = 1 block AES).

<img width="827" height="61" alt="image" src="https://github.com/user-attachments/assets/e8dc02a3-d37b-4535-888c-3c730760ae1d" />

Ví dụ đoạn này ,(*(_QWORD *)(a1 + 8) + v6 + 32) → chính là địa chỉ 1 byte trong state. Nó lấy byte này làm chỉ số (*(unsigned __int8 *)...). Tra cứu trong v7 (S-box), ở offset index + 16, sau đó ghi lại vào state, đây là các bước trong công đoạn SubBytes của AES.

Vì hàm này lặp 10 round, nên đây là AES-128

<img width="536" height="174" alt="image" src="https://github.com/user-attachments/assets/5a5f8501-4cf2-4294-afcd-95c5bf9662d9" />

<img width="628" height="383" alt="image" src="https://github.com/user-attachments/assets/f31ebe4e-4eea-43f9-8fe6-fabba990429d" />

Sửa lại tên các hàm 

<img width="377" height="103" alt="image" src="https://github.com/user-attachments/assets/eaf9f2b8-7262-47da-ae17-d149fed7f268" />

Ta thấy v12 chính là input block vì trong hàm aes_block_encrypt, nó copy input block state từ a2. Còn v8 thì là 1 aes context
