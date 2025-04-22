# DawgCTF2025 - Writeup Misc/Fwn
Link ctftime: https://ctftime.org/event/2651

![image](https://hackmd.io/_uploads/HymavmQyxe.png)

# Misc
## Look Long and Prosper
![image](https://hackmd.io/_uploads/S1BkOm7klg.png)
![image](https://hackmd.io/_uploads/ByiluX71lg.png)

Challenge này cho ta một dãy kí tự bị mã hóa bằng một thuật toán nào đó, yêu cầu tìm key thông qua một user có tên ***"Wikikenobi"***.

Tìm kiếm từ khóa này trên Google, nhận được một trang thông tin user của Wikipedia
![image](https://hackmd.io/_uploads/ByjOOmmJgl.png)
Tìm thấy user này bên trong https://en.wikipedia.org/wiki/Wikipedia:Database_reports/Potential_U5s/1
![image](https://hackmd.io/_uploads/SkfcuQ7ylx.png)

User này có một bài viết về bộ phim Star Trek, nhưng không nói rõ thuật toán mã hóa hay key nào được sử dụng. Check lại nội dung của bài **"The key is hidden in plain sight"** tức là key đã nằm trong đoạn text rồi.
![image](https://hackmd.io/_uploads/ryupumXkle.png)

Đến đây thì thử một vài cách và tìm ra được key dấu ở dạng chữ cái đầu tiên của mỗi câu.
Ghép lại và kết quả thu được key: **PADAWAN**

Tiếp theo, cần tìm ra thuật toán mã hóa ciphertext, "aiye_hoav_aqd_advi" là một chuỗi chỉ gồm các ký tự nên có thể là dạng mã hóa cổ điển
![image](https://hackmd.io/_uploads/rJ2Yq7Q1xg.png)
Có hai dạng phổ biến là Caesar và Vigenere, nhưng key là một chuỗi text thì Vigenere phù hợp hơn.
https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher

Vigenere Decode và thu được flag
![image](https://hackmd.io/_uploads/SyXesmXklg.png)

**FLAG: DawgCTF{live_long_and_edit}**

## Spectral Secrets
![image](https://hackmd.io/_uploads/r12IsXQyle.png)
![image](https://hackmd.io/_uploads/rkFDoXmkgx.png)

Resource: [System_of_a_Down_-Chop_Suey(HQ)_virus_free.wav](https://github.com/UMBCCyberDawgs/dawgctf-sp25/tree/main/Spectral%20Secrets)

Vì file .wav nên mở bằng Audacity.
![Screenshot 2025-04-19 173245](https://hackmd.io/_uploads/HJxM3QQJll.png)

Đối với các challenge liên quan đến wav, flag có thể được dấu dưới dạng [Spectrogram](https://en.wikipedia.org/wiki/Spectrogram), là một dạng biểu diễn trực quan của âm thanh dưới dạng phổ tần số theo thời gian.
Audacity hỗ trợ việc xem chế độ này.

![Screenshot 2025-04-19 173346](https://hackmd.io/_uploads/rkJ3nmmylx.png)

**FLAG: DawgCTF{4ud4c17y_my_b310v3d}**

# FWN
## Chunked Integrity (Forensics)
![image](https://hackmd.io/_uploads/HJoZ6XQklx.png)
![image](https://hackmd.io/_uploads/Bk7fpQQ1el.png)

Resource: [funnyCat.png](https://github.com/UMBCCyberDawgs/dawgctf-sp25/tree/main/Chunked%20Integrity)

Challenge cung cấp một bức ảnh png, khi mở ảnh lên đã thấy có phần bị lỗi không hiển thị hình ảnh, tức là có chunk lỗi trong ảnh.

![Screenshot 2025-04-19 145514](https://hackmd.io/_uploads/SJVO6QQklx.png)

Để phân tích cụ thể toàn bộ các chunk của file, sử dụng công cụ [pngcheck](http://www.libpng.org/pub/png/apps/pngcheck.html)

![Screenshot 2025-04-19 150415](https://hackmd.io/_uploads/B1-DAQ7yex.png)

Kết quả cho thấy chunk cuối của file đang không hợp lệ, và tên chunk đang là "EDAT", đây là tên chunk không hợp lệ trong png file.
Thông tin chi tiết về PNG chunk: http://www.libpng.org/pub/png/spec/1.2/PNG-Chunks.html

Vậy cần sửa byte "EDAT" thành "IDAT" để khôi phục lại chunk để hiển thị đầy đủ hình ảnh. Ở đây dùng HexEditor

![Screenshot 2025-04-19 150057](https://hackmd.io/_uploads/HJZW14m1ex.png)

Mở lại hình ảnh và lấy được flag trên ảnh.

![Screenshot 2025-04-19 150137](https://hackmd.io/_uploads/B1dQJ4XJll.png)

**FLAG: DawgCTF{C0rrup7_Chunkz}**

## My Aquarium (Web)
![image](https://hackmd.io/_uploads/rkuvJEQJle.png)
![image](https://hackmd.io/_uploads/H1yFJ47ygx.png)

Challenge này cung cấp một đường dẫn website đơn giản
![image](https://hackmd.io/_uploads/rJ9qy4m1lg.png)

Hai button "See Sea Animals" và "Take this quiz to find out what sea animal you are!" dẫn đến nội dung trang web bình thường

Button "Sea Animal Facts" lại dẫn đến một địa chỉ hoàn toàn khác
![image](https://hackmd.io/_uploads/BJQegVXyel.png)
Ở đây sử dụng trực tiếp resource từ Azure blob. Điều này dễ dẫn đến ["Azure Blob Storage Misconfiguration"](https://medium.com/@chatribingaurav/azure-blob-storage-misconfiguration-a-red-team-path-to-initial-access-c918689563bb)
![image](https://hackmd.io/_uploads/HJrJz4XJxl.png)

Sử dụng Azure Storage Explorer với tên "onlineaquarium", url "onlineaquarium.blob.core.windows.net/aquarium/resourses" để xem có resource container nào khác được public không. Kết quả tìm được file SecretFavoriteSeaAnimal.txt
![Screenshot 2025-04-19 184100](https://hackmd.io/_uploads/rJte-Nmkll.png)

**FLAG: DawgCTF{Bl0b_F15h_4re_S1lly}**

## pinpoint 1 (Forensics)
![image](https://hackmd.io/_uploads/r1RUGN7yxx.png)
![image](https://hackmd.io/_uploads/B1hPGN71lx.png)

Resource: [findMyPin.pcap](https://github.com/UMBCCyberDawgs/dawgctf-sp25/tree/main/pinpoint%201)

Dựa theo mô tả đề bài thì đây là file pcap của việc sử dụng plastic card và pin nhập vào USB.

![Screenshot 2025-04-19 155117](https://hackmd.io/_uploads/H1HiMVmklx.png)

Dựa trên file pcap cho thấy có các gói URB_BULK in/out. Thông tin về dạng này: https://osqa-ask.wireshark.org/questions/11054/analysing-usb-traffic/
![image](https://hackmd.io/_uploads/HJqz4EQkge.png)
Dữ liệu nằm trong phần "Leftover Capture Data"

![Screenshot 2025-04-19 161337](https://hackmd.io/_uploads/ryw2GNQJel.png)

Vì cần tìm mã pin nên ta sẽ filter các gói có source là host và các gói có dữ liệu
```
(usb.capdata) && (usb.src == 'host')
```

Sử dụng tshark để lấy dữ liệu từ phần "Leftiver Capture Data" gộp lại thành một file chứa các hex data liên tiếp nhau.
Sau đó dùng `xxd -r -p raw pin.bin` để chuyển dữ liệu hex thuần túy (plaintext hex) trong file raw thành file nhị phân (binary) tên pin.bin
Đọc file pin.bin ta thu được ba mã pin đã được sử dụng. Thử check lần lượt ba mã và thu được flag

![Screenshot 2025-04-19 163604](https://hackmd.io/_uploads/HJY6zN7kll.png)

**FLAG: DawgCTF{7901}**









