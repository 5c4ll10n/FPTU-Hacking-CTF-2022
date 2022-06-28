# Broken
* File: *[flag.png](https://github.com/5c4ll10n/FPTU-Hacking-CTF-2022/blob/main/Data/Readme)*
* Download ảnh từ đề của ban tổ chức về thì mình thấy đó là 1 image như tên đề là broken nghĩa là nó không mở được nên mình đã dùng hexit để xem các giá trị trong header của nó. Lướt nhanh quá thì thấy được đây là 1 file image có extension là .png nên mình check xem header của nó đã đúng chưa

![](/Data/Broken1.png)

* So sánh với header của 1 file png bình thường thì file này không có phần “IHDR” và đã bị thay bằng “FPTU” ở vị trí của IHDR. Và 1 điều đáng chú ý nữa là height và width của image này đã bị chỉnh về 0x0. Sửa lại file từ “FPTU” thành IHDR và tăng height và width của bức ảnh lên thì mình được 1 image như sau:

![](/Data/Broken2.png)

* Mình nhận thấy những chấm màu kia có vẻ như là flag nhưng vì height và width có vẻ như không đúng nên không thể hiện ra chính xác được. nên mình đã brute-force dimension của image này với đoạn code như sau:
``` python
from zlib import crc32

data = open("flag2.png",'rb').read()
index = 12

ihdr = bytearray(data[index:index+17])
width_index = 7
height_index = 11

for x in range(1,2000):
	height = bytearray(x.to_bytes(2,'big'))
	for y in range(1,2000):
		width = bytearray(y.to_bytes(2,'big'))
		for i in range(len(height)):
			ihdr[height_index - i] = height[-i -1]
		for i in range(len(width)):
			ihdr[width_index - i] = width[-i -1]
		if hex(crc32(ihdr)) == '0x612055aa':
			print("width: {} height: {}".format(width.hex(),height.hex()))
	for i in range(len(width)):
			ihdr[width_index - i] = bytearray(b'\x00')[0]
```
* Vì khi thay đổi height và weight cũng sẽ làm thay đổi CRC chunk nên mình lấy CRC chunk từ file orginal làm mốc để tìm ra height và width đúng của file image.
* Dùng hexit thay đổi height và width của image ta sẽ được 1 image hoàn chỉnh và flag hiện lên:
![](/Data/Broken3.png)
