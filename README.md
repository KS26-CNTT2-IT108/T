YÊU CẦU:

Nêu các bước lọc file .log

đọc tệp -> lọc các từ khóa -> đếm ra kết quả



1 Giới thiệu.

&#x20; Phòng DevOps cần thống kê nhanh: \*"Có bao nhiêu sự cố TIMEOUT xảy ra trong tệp log hôm nay?"\*. Yêu cầu kết quả trả về phải là một con số tổng duy nhất thay vì in ra hàng nghìn dòng văn bản.

2 Phân tích.

\- các bước làm.

* Liệt kê danh sách
* lọc những file có chứa " NOT TIMEOUT"
* Tìm những file có chứa "TIMEOUT"
* Đếm

3 Giải.

Từng dòng lệch:powershell



* get-childitem 
* get-childitem -r -filter \*.log
* get-childitem -r -filter \*.log | select-string "TIMEOUT"
* get-childitem -r -filter \*.log | select-string "TIMEOUT" | where-object { $\_ -notmatch "NOT TIME" }
* (get-childitem -r -filter \*.log | select-string "TIMEOUT" | where-object { $\_ -notmatch "NOT TIME" }).count



Một dòng lệch:powrshell

* (get-childitem -r -filter \*.log | select-string "TIMEOUT" | where-object { $\_ -notmatch "NOT TIME" }).count



* get-childitem là lệch dùng để liệt kê các fike con trong thư mục
* &#x20;-r là viết tắt của -recurse là đi vào cả các thư mục con
* \-filter \*.log: chỉ giữ lại những file có tên kết thúc bằng .log. Dấu \* nghĩa là bất kỳ tên nào.
* |: chuyển kết quả của lệnh bên trái sang làm đầu vào cho lệnh bên phải
* Select-String "TIMEOUT": mở từng file nhận được và tìm dòng nào có chữ TIMEOUT.
* $\_: đại diện cho mục đang được xét, ở đây là từng dòng tìm được ở bước trước.
* \-notmatch "NOT TIME": điều kiện không khớp với. Dòng nào có chứa NOT TIME sẽ bị bỏ đi, dòng còn lại được giữ.
* .Count là thuộc tính đếm số lượng mục có trong kết quả của một lệnh PowerShell.

4 So sánh và kết luận 



* Liệt kê file: Get-ChildItem <=> ls
* Tìm file .log (cả thư mục con): Get-ChildItem -r -filter \*.log <=> find . -name "\*.log"
* Tìm dòng có “TIMEOUT”: ... | Select-String "TIMEOUT" <=> grep -r "TIMEOUT" --include="\*.log" .
* Bỏ dòng có “NOT TIME”: ... | Where-Object { $\_ -notmatch "NOT TIME" } <=> ... | grep -v "NOT TIME"
* Đếm số dòng: (...).Count <=> ... | wc -l



Giống nhau: cùng chia bước (liệt kê, tìm, lọc, đếm), cùng dùng pipe |, cùng ra một kết quả.



Khác nhau: PowerShell đặt tên dài, dễ hiểu nghĩa và truyền đối tượng; Git Bash đặt tên ngắn, gõ nhanh hơn và truyền chữ thuần.



