#characters
Trong challenge này có 2 file:
  - enc.py: thực thi mã hóa flag
  - out.txt: kết quả mã hóa

Từ file out.txt ta có thể thấy ct (ciphertext), n, e. Và từ file enc.py có thể thấy đây là mã hóa RSA

1. enc.py
	Có thể thấy tác giả đã lưu flag thành 1 list và mã hóa rồi lưu từng kí tự vào ct[]:
		flag = list(flag)
		ct = []
		for i in flag:
   	 		ct.append( pow(  bytes_to_long(i.encode()),e,n) )
			
2. Solve
	Em đã thử decode để xem kết quả trả về gì thì có kí tự đầu tiên của flag "F"
	![image](https://user-images.githubusercontent.com/102909809/176236194-612d0b9d-f2a7-44a0-b0db-b1d80f17567d.png)
	Decode từng phần tử của list sẽ thu được flag:
	
	FPTUHacking{3ncRYpt1ng_34ch_ch4r_ainT_G0nn4_h3lp}
	
