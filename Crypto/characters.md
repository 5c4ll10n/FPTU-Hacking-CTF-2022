# characters

Trong challenge này có 2 file:
  - [enc.py](/Data/enc.py): thực thi mã hóa flag
  - [out.txt](/Data/out.txt): kết quả mã hóa

Từ file out.txt ta có thể thấy ct (ciphertext), n, e. Và từ file enc.py có thể thấy đây là mã hóa RSA

1. enc.py

	Có thể thấy tác giả đã lưu flag thành 1 list và mã hóa rồi lưu từng kí tự vào ct[]:
		
		flag = list(flag)
		ct = []
		for i in flag:
   	 		ct.append( pow(  bytes_to_long(i.encode()),e,n) )
			
2. Solve


	Em đã thử decode phần tử đầu của list để xem kết quả trả về gì thì có kí tự đầu tiên của flag "F"
	![image](https://user-images.githubusercontent.com/102909809/176236194-612d0b9d-f2a7-44a0-b0db-b1d80f17567d.png)
	Code để decode từng phần tử của list: 
	![image](https://user-images.githubusercontent.com/103638792/176424371-59623fb2-6a77-41f4-93b1-5b5aa1cb2409.png)
	Kết quả khi chạy đoạn code trên:
	![image](https://user-images.githubusercontent.com/103638792/176424608-1d0a8ee5-cff1-4a97-aada-70bd081d1a7d.png)
	Sau đó thì sẽ là công việc hơi thổ dân 1 chút là thu thập từng kí tự một :<
	
### Flag: FPTUHacking{3ncRYpt1ng_34ch_ch4r_ainT_G0nn4_h3lp}
	
