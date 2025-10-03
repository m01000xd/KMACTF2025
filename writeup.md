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

<img width="602" height="301" alt="image" src="https://github.com/user-attachments/assets/ea87ca46-51c2-45b3-92b3-0fd5b04d8d20" />

Xref thì thấy v8 được gọi từ hàm sub_7FF622A26E20

Hàm này return từ hàm sub_7FF622A26EC0 
<img width="1138" height="576" alt="image" src="https://github.com/user-attachments/assets/acac4bf4-7683-4dc4-b813-168997597b24" />

<img width="944" height="584" alt="image" src="https://github.com/user-attachments/assets/8ddd6554-983c-45a4-8beb-0d721a6bcc86" />

<img width="989" height="312" alt="image" src="https://github.com/user-attachments/assets/d5547084-f0ab-4ec8-b2c7-6451ea793deb" />

Hàm này chính là key expansion trong AES

<img width="863" height="256" alt="image" src="https://github.com/user-attachments/assets/0c65b4cb-e76c-4103-83e2-f8fcddb72fc6" />

Phần đầu copy 16 byte key từ struct tại địa chỉ a1+24

<img width="546" height="148" alt="image" src="https://github.com/user-attachments/assets/b621a9d8-f380-4256-8b37-5228f0e8ad41" />

Phần tiếp theo là RotWord

<img width="615" height="156" alt="image" src="https://github.com/user-attachments/assets/578e3440-8b96-4a2a-99d9-8152ec647cfa" />


<img width="1091" height="25" alt="image" src="https://github.com/user-attachments/assets/45c1eff9-25e6-4334-9b78-ed43fc15dba9" />


Phần tiếp theo là SubWord thay thế 1 byte dùng hộp thế Sbox lấy từ struct qword_7FF622AD9490

<img width="851" height="241" alt="image" src="https://github.com/user-attachments/assets/c7eab53f-fb96-48a9-9670-248a9b3d328d" />

Tiếp theo là xor với hằng số vòng Rcon

<img width="236" height="34" alt="image" src="https://github.com/user-attachments/assets/52c86acb-93c5-439b-a404-7b2f81929e77" />

AES-128 cần expand đến 176 bytes(11 rounds x 16 byte)

<img width="320" height="25" alt="image" src="https://github.com/user-attachments/assets/d7688149-85c8-4a67-8dd9-fb7e097431dc" />

Rename lại hàm, key được truyền làm tham số thứ 2

<img width="372" height="48" alt="image" src="https://github.com/user-attachments/assets/aa1e51f5-66fe-40f5-bbaf-93e84e75aebd" />

Key được cấp phát 1 vùng nhớ 16 byte, sau đó được hardcode = mảng xmmword_7FF622A9A970

<img width="498" height="28" alt="image" src="https://github.com/user-attachments/assets/0c410bcc-f7c1-4ebd-bf9b-bf960074e232" />

Ta sẽ rename lại các hàm đã phân tích để luồng chương trình rõ ràng hơn

<img width="985" height="575" alt="image" src="https://github.com/user-attachments/assets/8e7bbb0c-27f6-4d43-8bde-e2be6f61d892" />

<img width="980" height="543" alt="image" src="https://github.com/user-attachments/assets/e0f83ff6-d54a-448b-9f36-2fbddb58ea65" />

Ta thấy không có phần tạo IV nên ta có thể khẳng định đây là AES-128 mode ECB

Chương trình sẽ mã hóa input nhập vào, padding nếu độ dài input không chia hết cho 16, sau đó tạo key expansion cho mỗi round rồi gọi hàm aes_block_encrypt để mã hóa mỗi block input, input sau khi mã hóa sẽ dài 80 byte, đem đi so với cipher chính là mảng encrypted_flag_data

<img width="530" height="491" alt="image" src="https://github.com/user-attachments/assets/69febb24-808d-4ebb-9541-4f48845d9c88" />

Nếu khớp thì in ra cmd Correct. Có key, có cipher, giờ ta sẽ viết script để lấy flag.

Việc đầu tiên là ta phải lấy được Sbox

Ta nhớ lại Sbox được lấy từ struct qword_7FF622AD9490

<img width="711" height="243" alt="image" src="https://github.com/user-attachments/assets/ec2e9015-dbcd-4e6a-8f0d-ee194e209dc3" />

<img width="600" height="44" alt="image" src="https://github.com/user-attachments/assets/e89dd6a0-57c1-4255-af49-338859d2461e" />

v2 lúc đầu gán = qword_7FF622AD9490, sau đó ảnh trên gán v7 = giá trị của 1 con trỏ qword tại địa chỉ v2 + 8, rồi phần kiểm tra kiểm tra độ dài của Sbox tại giá trị của 1 con trỏ DWORD tại địa chỉ v7 + 8

<img width="644" height="21" alt="image" src="https://github.com/user-attachments/assets/a74a1eb5-9421-4953-9f8a-8afd4d645ff0" />

Phần này sẽ lấy giá trị của 1 byte Sbox tại địa chỉ v7 + 16.

<img width="609" height="40" alt="image" src="https://github.com/user-attachments/assets/37150dd6-7ba5-4585-b5ab-579009bafaf9" />

Đầu tiên qword_7FF622AD9490 chứa 1 giá trị là con trỏ 

<img width="478" height="234" alt="image" src="https://github.com/user-attachments/assets/8530e5cc-fe0c-4d13-9387-fbcf1e3d6837" />

Nó sẽ lấy 1 con trỏ tới struct tại QWORD (giá trị của nó+8)

<img width="591" height="108" alt="image" src="https://github.com/user-attachments/assets/b44e75b6-3c4d-4994-80b8-06bd718e968e" />

Tức con trỏ tới struct đó có địa chỉ chính là 8 byte sau tính từ 8 byte đầu, là địa chỉ 7ff622a55658

<img width="551" height="198" alt="image" src="https://github.com/user-attachments/assets/07f9d845-57a9-4ad8-892d-bbf4194298b4" />

G để nhảy tới địa chỉ này. Sau đó nó lấy độ dài của Sbox là 1 DWORD tính từ địa chỉ của nó + 8

<img width="421" height="65" alt="image" src="https://github.com/user-attachments/assets/d71d13c8-e0cb-4976-b8f7-7d3244f6eb70" />

Tức là đây, như hình thì Sbox có độ dài 0x100 = 256 byte

Tiếp theo để truy cập tới byte đầu của Sbox thì tại địa chỉ đó + thêm 8

<img width="517" height="171" alt="image" src="https://github.com/user-attachments/assets/e410be2e-74c9-406c-8316-8242ed251c92" />

Tức là byte 0xde này là byte đầu. Vậy ta sẽ lấy 256 byte kể từ địa chỉ này trở xuống thì sẽ lấy được sbox
```python3
# Custom S-box bạn cung cấp
CUSTOM_SBOX = [
     0xDE, 0x1C, 0x5B, 0x07, 0x0D, 0x3F, 0xD0, 0xBF, 0xC2, 0x82, 
  0x40, 0x25, 0xF4, 0x7E, 0x93, 0xDD, 0x39, 0x05, 0x6A, 0xFC, 
  0x68, 0x5A, 0x9B, 0x7D, 0x09, 0xB1, 0xCA, 0xE4, 0xAE, 0x37, 
  0x6C, 0x62, 0xAF, 0xB2, 0xC1, 0x02, 0x75, 0x71, 0xF1, 0xCD, 
  0xA3, 0x0B, 0xBB, 0xA0, 0x2B, 0x38, 0x29, 0x7B, 0xFB, 0x1B, 
  0x01, 0x4D, 0x17, 0x8E, 0xCC, 0x16, 0xA4, 0xA7, 0xE3, 0x34, 
  0x12, 0x91, 0x9A, 0x9C, 0x30, 0x90, 0x31, 0x3D, 0x61, 0x96, 
  0x6B, 0x3C, 0x15, 0x54, 0x74, 0x84, 0x21, 0xB5, 0x85, 0xE2, 
  0xD8, 0xB0, 0x44, 0x3A, 0x7C, 0x20, 0x0C, 0x78, 0xFE, 0xC4, 
  0x51, 0xAB, 0xDA, 0xB6, 0x70, 0xEB, 0xE1, 0x22, 0xD9, 0x8B, 
  0x72, 0xCF, 0xEE, 0x2A, 0xEF, 0xE8, 0xE0, 0x35, 0x65, 0x32, 
  0xEC, 0x66, 0x5F, 0xD3, 0xB8, 0x77, 0x33, 0xF0, 0x92, 0x46, 
  0x58, 0xB9, 0xD5, 0xF6, 0xC5, 0xBA, 0xDB, 0xB3, 0x2D, 0x3E, 
  0x7F, 0x04, 0xC7, 0x6D, 0xA2, 0xF3, 0xCB, 0xFD, 0x9D, 0x5D, 
  0x98, 0x67, 0x69, 0x6F, 0xFA, 0x0A, 0xA1, 0x95, 0x80, 0x59, 
  0x1A, 0x03, 0x97, 0x83, 0x4A, 0x11, 0x9E, 0xC8, 0x06, 0x24, 
  0xB4, 0x08, 0xF2, 0xC9, 0x10, 0xD2, 0x1E, 0xBD, 0xAA, 0x9F, 
  0xD1, 0x26, 0x2E, 0xE5, 0x0F, 0x60, 0xF7, 0xA5, 0x79, 0xC3, 
  0x76, 0x87, 0x43, 0x5E, 0x42, 0x47, 0x86, 0x2F, 0x3B, 0x56, 
  0x8A, 0x41, 0x53, 0x81, 0x50, 0x8D, 0x4B, 0xA9, 0x27, 0x4E, 
  0x13, 0x4F, 0x2C, 0x63, 0x48, 0x49, 0xD6, 0xB7, 0x28, 0x6E, 
  0xDC, 0x5C, 0x23, 0xEA, 0xBE, 0x73, 0xE9, 0x14, 0x1D, 0x8F, 
  0x4C, 0xFF, 0xE6, 0x64, 0x94, 0x1F, 0xC0, 0xD7, 0xBC, 0x55, 
  0xC6, 0xDF, 0xAD, 0x7A, 0x0E, 0x19, 0xA8, 0x89, 0x57, 0x36, 
  0x45, 0xE7, 0xF9, 0xCE, 0xF8, 0x8C, 0x99, 0xF5, 0x18, 0xA6, 
  0x52, 0xED, 0xAC, 0xD4, 0x88, 0x00
]

# RCON chuẩn AES
RCON = [0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x1B,0x36]

# Encrypted flag
encrypted_flag_data = bytes([
  138,  35,  34,109,219,124,24,180,34,232,
  32,86,13,87,85,44,72,16,12,70,
  161,143,105,99,203,208,242,171,97,139,
  101,130,28,149,11,241,20,6,34,145,
  41,92,19,95,252,199,196,160,109,167,
  250,128,4,139,239,178,232,200,172,56,
   36,124,121,244,24,72,61,58,63,253,
  222,172,24,176,68,177,93,162,224,166
])

# Key từ xmmword (thứ tự đảo ngược)
key_hex = "746320467597F7ABA6D2AE2816157E2B"
key_bytes = bytes.fromhex(key_hex)[::-1]

# Hàm phụ trợ AES (sử dụng custom S-box)
def aes_decrypt_block(block, round_keys, sbox=CUSTOM_SBOX):
    state = list(block)
    INV = [0]*256
    for i,b in enumerate(sbox): INV[b]=i

    def add_round_key(state, rk):
        for i in range(16): state[i] ^= rk[i]
    def sub_bytes(state):
        for i in range(16): state[i] = sbox[state[i]]
    def inv_sub_bytes(state):
        for i in range(16): state[i] = INV[state[i]]
    def shift_rows(state):
        tmp = state.copy()
        for r in range(4):
            for c in range(4):
                state[r + 4*c] = tmp[r + 4*((c - r) % 4)]
    def mul(a,b):
        res=0
        for _ in range(8):
            if b&1: res^=a
            carry=a&0x80
            a=(a<<1)&0xFF
            if carry: a^=0x1B
            b>>=1
        return res
    def mix_single_column(a):
        t = a[0] ^ a[1] ^ a[2] ^ a[3]
        return [
            a[0]^t^mul(a[0]^a[1],0x02),
            a[1]^t^mul(a[1]^a[2],0x02),
            a[2]^t^mul(a[2]^a[3],0x02),
            a[3]^t^mul(a[3]^a[0],0x02)
        ]
    def mix_columns(state):
        for c in range(4):
            col=[state[r+4*c] for r in range(4)]
            mc=mix_single_column(col)
            for r in range(4): state[r+4*c]=mc[r]

    # Key expansion
    def key_expansion(key_bytes):
        Nk=4; Nr=10
        w=[0]*(4*(Nr+1))
        for i in range(Nk):
            w[i]=(key_bytes[4*i]<<24)|(key_bytes[4*i+1]<<16)|(key_bytes[4*i+2]<<8)|key_bytes[4*i+3]
        for i in range(Nk,4*(Nr+1)):
            temp=w[i-1]
            if i%Nk==0:
                bts=[(temp>>24)&0xFF,(temp>>16)&0xFF,(temp>>8)&0xFF,temp&0xFF]
                bts=bts[1:]+bts[:1]
                bts=[sbox[b] for b in bts]
                bts[0]^=RCON[i//Nk-1]
                temp=(bts[0]<<24)|(bts[1]<<16)|(bts[2]<<8)|bts[3]
            w[i]=w[i-Nk]^temp
        round_keys=[]
        for r in range(Nr+1):
            rk=[]
            for j in range(4):
                word=w[4*r+j]
                rk.extend([(word>>24)&0xFF,(word>>16)&0xFF,(word>>8)&0xFF,word&0xFF])
            round_keys.append(rk)
        return round_keys

    rk = key_expansion(key_bytes)

    # AES decrypt block
    state=list(block)
    Nr=10
    add_round_key(state,rk[Nr])
    for r in range(Nr-1,0,-1):
        shift_rows(state)
        inv_sub_bytes(state)
        add_round_key(state,rk[r])
        mix_columns(state)
    shift_rows(state)
    inv_sub_bytes(state)
    add_round_key(state,rk[0])
    return bytes(state)

# Giải mã tất cả block 16 byte
plaintext=b""
for i in range(0,len(encrypted_flag_data),16):
    block=encrypted_flag_data[i:i+16]
    plaintext+=aes_decrypt_block(block,key_bytes)

# Loại PKCS#7 padding
pad=plaintext[-1]
plaintext=plaintext[:-pad]

print("Flag:", plaintext.decode())

```

Nhưng khi ta chạy thì chỉ ra được 1 chuỗi byte rác

```python3
b'\xbe\xd4a\x85~T\xf8\xa0aiU\xbd\x04\xee\xe5\xca\xcc\xa4Vw\x92\xdb\x8e\xcb\x88\x06\xa2[Dcl\t\x01\x93)&\xd9\x9ea=.\xd3\xc8p\x13\x84Y+\x859\x90\x94\x83ch!i\x9d\xfc-\x9fT\xb2\x0c\xf8\xccCS\x05\x89\x00\xfc\xf8QR\x19\x8b\x86j\xf9'
```

Có vẻ như đã sai ở đâu đó, có thể Sbox đã sai chăng, hoặc bị sửa ở đâu đó

<img width="690" height="233" alt="image" src="https://github.com/user-attachments/assets/fd58e0aa-db1f-4075-9a6a-2b12277682eb" />

Thử xref đến struct chứa sbox qword_7FF622AD9490

<img width="690" height="238" alt="image" src="https://github.com/user-attachments/assets/2c38b58c-bc75-4c31-92cf-af8787ba6176" />

Ta thấy ngoài được gọi trong hàm SubBytes và key_expansion, nó còn được gọi ngay đầu tiên bởi 1 hàm sub_7FF622A26AC0

<img width="733" height="382" alt="image" src="https://github.com/user-attachments/assets/4887dea8-8bd3-4496-b058-8d2d2060913b" />

<img width="635" height="393" alt="image" src="https://github.com/user-attachments/assets/41555196-fac6-4f9c-80da-c05ad1c84c7b" />

<img width="519" height="171" alt="image" src="https://github.com/user-attachments/assets/ebf168d9-fda1-4531-ac15-d566dfecf61e" />

Hàm này dựa vào kết quả return từ hàm sub_7FF622A26A70 mà sẽ hoán đổi vị trí 1 số byte của Sbox. Thử nhảy vào hàm sub_7FF622A26A70 xem nó làm gì

<img width="486" height="249" alt="image" src="https://github.com/user-attachments/assets/fda218eb-9e64-4ce3-9069-2188eea32208" />

Hàm này gọi IsDebuggerPresent để kiểm tra có bị debug không, nếu có thì return 1. Tức là khi ta debug nó đã sửa sbox của chúng ta. Vậy để bypass anti-debug này ta sẽ đặt bp ở hàm sub_7FF622A26AC0, khi nào gọi qua hàm sub_7FF622A26A70 thì sửa lại eax = 0.

<img width="1018" height="335" alt="image" src="https://github.com/user-attachments/assets/b9db9c90-91e8-48bf-9bbe-741f8e3f15b5" />

<img width="564" height="145" alt="image" src="https://github.com/user-attachments/assets/a8855efc-a570-4473-89a1-41d4359fee4a" />

<img width="726" height="353" alt="image" src="https://github.com/user-attachments/assets/598f085d-7eb2-4368-bfb3-ea6e9929a577" />

Đã nhảy đến nhánh không debug. Nhánh không debug cũng swap 1 vài vị trí trong sbox nên ta sẽ chạy đến retn rồi dump sbox ra

```python3
 sbox = [0xDE, 0x1C, 0x5B, 0x07, 0x0D, 0x3F, 0xD0, 0xBF, 0xC2, 0x82, 
  0x40, 0x25, 0xF4, 0x7E, 0x93, 0xDD, 0x39, 0x05, 0x6A, 0x16, 
  0x68, 0x5A, 0x9B, 0x7D, 0x09, 0xB1, 0xCA, 0xE4, 0xAE, 0x37, 
  0x6C, 0x62, 0xAF, 0xB2, 0xC1, 0x02, 0x75, 0x71, 0xF1, 0xCD, 
  0xA3, 0x0B, 0xBB, 0xA0, 0x2B, 0x38, 0x29, 0x7B, 0xFB, 0x1B, 
  0x01, 0x4D, 0x17, 0x8E, 0xCC, 0xFC, 0xA4, 0xA7, 0xE3, 0x34, 
  0x12, 0x91, 0x9A, 0x9C, 0x30, 0x90, 0x31, 0x3D, 0x61, 0x96, 
  0x6B, 0x3C, 0x15, 0x54, 0x74, 0x84, 0x21, 0xB5, 0x85, 0xE2, 
  0xD8, 0xB0, 0x44, 0x3A, 0x7C, 0x20, 0x0C, 0x78, 0xFE, 0xC4, 
  0x51, 0xAB, 0xDA, 0xB6, 0x70, 0xEB, 0xE1, 0x22, 0xD9, 0x8B, 
  0x72, 0xCF, 0xEE, 0x2A, 0xEF, 0xE8, 0xE0, 0x35, 0x65, 0x32, 
  0xEC, 0x66, 0x5F, 0xD3, 0xB8, 0x77, 0x33, 0xF0, 0x92, 0x46, 
  0x58, 0xB9, 0xD5, 0xF6, 0xC5, 0xBA, 0xDB, 0xB3, 0x2D, 0x3E, 
  0x7F, 0x04, 0xC7, 0x6D, 0xA2, 0xF3, 0xCB, 0xFD, 0x9D, 0x5D, 
  0x98, 0x67, 0x69, 0x6F, 0xFA, 0x0A, 0xA1, 0x95, 0x80, 0x59, 
  0x1A, 0x03, 0x97, 0x83, 0x4A, 0x11, 0x9E, 0xC8, 0x06, 0x24, 
  0xB4, 0x08, 0xF2, 0xC9, 0x10, 0xD2, 0x1E, 0xBD, 0xAA, 0x9F, 
  0xD1, 0x26, 0x2E, 0xE6, 0x0F, 0x60, 0xF7, 0xA5, 0x79, 0xC3, 
  0x76, 0x87, 0x43, 0x5E, 0x42, 0x47, 0x86, 0x2F, 0x3B, 0x56, 
  0x8A, 0x41, 0x53, 0x81, 0x50, 0x8D, 0x4B, 0xA9, 0x27, 0x4E, 
  0x13, 0x4F, 0x88, 0x63, 0x48, 0x49, 0xD6, 0xB7, 0x28, 0x6E, 
  0xDC, 0x5C, 0x23, 0xEA, 0xBE, 0x73, 0xE9, 0x14, 0x1D, 0x8F, 
  0x4C, 0xFF, 0xE5, 0x64, 0x94, 0x1F, 0xC0, 0xD7, 0xBC, 0x55, 
  0xC6, 0xDF, 0xAD, 0x7A, 0x0E, 0x19, 0xA8, 0x89, 0x57, 0x36, 
  0x45, 0xE7, 0xF9, 0xCE, 0xF8, 0x8C, 0x99, 0xF5, 0x18, 0xA6, 
  0x52, 0xED, 0xAC, 0xD4, 0x2C, 0x00]
```
```python3
# -*- coding: utf-8 -*-
CUSTOM_SBOX = [
    222,28,91,7,13,63,208,191,194,130,64,37,244,126,147,221,
    57,5,106,22,104,90,155,125,9,177,202,228,174,55,108,98,
    175,178,193,2,117,113,241,205,163,11,187,160,43,56,41,123,
    251,27,1,77,23,142,204,252,164,167,227,52,18,145,154,156,
    48,144,49,61,97,150,107,60,21,84,116,132,33,181,133,226,
    216,176,68,58,124,32,12,120,254,196,81,171,218,182,112,235,
    225,34,217,139,114,207,238,42,239,232,224,53,101,50,236,102,
    95,211,184,119,51,240,146,70,88,185,213,246,197,186,219,179,
    45,62,127,4,199,109,162,243,203,253,157,93,152,103,105,111,
    250,10,161,149,128,89,26,3,151,131,74,17,158,200,6,36,
    180,8,242,201,16,210,30,189,170,159,209,38,46,230,15,96,
    247,165,121,195,118,135,67,94,66,71,134,47,59,86,138,65,
    83,129,80,141,75,169,39,78,19,79,136,99,72,73,214,183,
    40,110,220,92,35,234,190,115,233,20,29,143,76,255,229,100,
    148,31,192,215,188,85,198,223,173,122,14,25,168,137,87,54,
    69,231,249,206,248,140,153,245,24,166,82,237,172,212,44,0
]

RCON = [0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x1B,0x36]

encrypted_flag_data = bytes([
 138,35,34,109,219,124,24,180,34,232,32,86,13,87,85,44,
 72,16,12,70,161,143,105,99,203,208,242,171,97,139,101,130,
 28,149,11,241,20,6,34,145,41,92,19,95,252,199,196,160,109,
 167,250,128,4,139,239,178,232,200,172,56,36,124,121,244,24,
 72,61,58,63,253,222,172,24,176,68,177,93,162,224,166
])

# key (hex from xmmword) reversed as little-endian
key_hex = "746320467597F7ABA6D2AE2816157E2B"
key_bytes = bytes.fromhex(key_hex)[::-1]

# AES helper routines using CUSTOM_SBOX in key expansion and SubBytes
def xtime(a): return ((a<<1)^0x1B)&0xFF if (a&0x80) else (a<<1)&0xFF
def mul(a,b):
    res=0
    for _ in range(8):
        if b&1: res ^= a
        carry = a & 0x80
        a = (a << 1) & 0xFF
        if carry: a ^= 0x1B
        b >>= 1
    return res & 0xFF

def key_expansion(key_bytes, sbox=CUSTOM_SBOX):
    Nk=4; Nr=10
    w=[0]*(4*(Nr+1))
    for i in range(Nk):
        w[i] = (key_bytes[4*i]<<24)|(key_bytes[4*i+1]<<16)|(key_bytes[4*i+2]<<8)|key_bytes[4*i+3]
    for i in range(Nk, 4*(Nr+1)):
        temp = w[i-1]
        if i % Nk == 0:
            bts = [ (temp>>24)&0xFF, (temp>>16)&0xFF, (temp>>8)&0xFF, temp&0xFF ]
            # RotWord
            bts = bts[1:]+bts[:1]
            # SubWord with custom sbox
            bts = [ sbox[b] for b in bts ]
            bts[0] ^= RCON[(i//Nk)-1]
            temp = (bts[0]<<24)|(bts[1]<<16)|(bts[2]<<8)|bts[3]
        w[i] = w[i-Nk] ^ temp
    round_keys=[]
    for r in range(Nr+1):
        rk=[]
        for j in range(4):
            word = w[4*r + j]
            rk += [ (word>>24)&0xFF, (word>>16)&0xFF, (word>>8)&0xFF, word&0xFF ]
        round_keys.append(rk)
    return round_keys

def inv_sbox_from(sbox):
    inv=[0]*256
    for i,b in enumerate(sbox): inv[b]=i
    return inv

def inv_shift_rows(state):
    tmp = state[:]
    for r in range(4):
        for c in range(4):
            state[r + 4*c] = tmp[r + 4*((c - r) % 4)]

def inv_sub_bytes(state, inv_sbox):
    for i in range(16): state[i] = inv_sbox[state[i]]

def inv_mix_single_column(a):
    return [
        mul(a[0],0x0e) ^ mul(a[1],0x0b) ^ mul(a[2],0x0d) ^ mul(a[3],0x09),
        mul(a[0],0x09) ^ mul(a[1],0x0e) ^ mul(a[2],0x0b) ^ mul(a[3],0x0d),
        mul(a[0],0x0d) ^ mul(a[1],0x09) ^ mul(a[2],0x0e) ^ mul(a[3],0x0b),
        mul(a[0],0x0b) ^ mul(a[1],0x0d) ^ mul(a[2],0x09) ^ mul(a[3],0x0e)
    ]

def inv_mix_columns(state):
    for c in range(4):
        col = [ state[r + 4*c] for r in range(4) ]
        im = inv_mix_single_column(col)
        for r in range(4): state[r + 4*c] = im[r] & 0xFF

def add_round_key(state, rk):
    for i in range(16): state[i] ^= rk[i]

def aes_decrypt_block(block, round_keys, sbox=CUSTOM_SBOX):
    inv_s = inv_sbox_from(sbox)
    state = list(block)
    Nr = 10
    add_round_key(state, round_keys[Nr])
    for r in range(Nr-1, 0, -1):
        inv_shift_rows(state)
        inv_sub_bytes(state, inv_s)
        add_round_key(state, round_keys[r])
        inv_mix_columns(state)
    inv_shift_rows(state)
    inv_sub_bytes(state, inv_s)
    add_round_key(state, round_keys[0])
    return bytes(state)

# Run decryption
rk = key_expansion(key_bytes, CUSTOM_SBOX)
pt = b''
for i in range(0, len(encrypted_flag_data), 16):
    pt += aes_decrypt_block(encrypted_flag_data[i:i+16], rk, CUSTOM_SBOX)

# PKCS#7 unpad
pad = pt[-1]
if 1 <= pad <= 16 and pt.endswith(bytes([pad])*pad):
    pt = pt[:-pad]

print("Flag:", pt.decode('utf-8', errors='replace'))
```

```python3
Flag:KMACTF{97baa5e0a73cba5e114e5d4024dac31a1d6eb7d8964c4f669389482cebee94db}
```
