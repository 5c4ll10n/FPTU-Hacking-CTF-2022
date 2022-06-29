# RRSSAA

Trong challenge có gắn kèm 2 file:
	
	- enc.py để thực hiện việc mã hóa flag
	- output.txt chứa kết quả mã hóa
Từ source code có thể thấy flag đã được chia làm 2 phần mà mã hóa từng phần một

Phần flag c1:

    Có thể thấy tác giả đã gán số nguyên p, q thành 1 số nguyên tố 512 bits duy nhất và mũ 4 số đó lên để thành số n1
  ![image](https://user-images.githubusercontent.com/102909809/176015725-009dcbd9-9ee0-4a79-8e81-36ede3b1915c.png)

	Theo lý thuyết RSA thì số n = p*q. Vậy nên em lấy số n bằng √n1
  ![image](https://user-images.githubusercontent.com/102909809/176015748-0f0a6100-5a8a-43e8-b8c1-df9f918b6630.png)

	Có n, p, q, e, ct1 rồi nên em dùng dcode.com để decode phần đầu của flag. Và có phần 1 của flag: FPTUHacking{Us3_0f_s1ngl3_pr1m3_4nd
  ![image](https://user-images.githubusercontent.com/102909809/176015781-1bbc9ee0-9887-40da-a5d7-d582dc5b9c19.png)
	

Phần flag c2:

	Em sử dụng RsaCtfTool để decode phần 2
    ![image](https://user-images.githubusercontent.com/102909809/176016276-f3e6fe4c-8c14-407e-a8df-dd4135b1e2fe.png)

  ![image](https://user-images.githubusercontent.com/102909809/176015813-1979e5af-6c43-47ab-8b0d-03d4ff894585.png)
    
    Và có được phần còn lại của flag: _cl0se_prim3s_w3r3_w4st3_0f_c0mput3r_Cycl35}
Flag: FPTUHacking{Us3_0f_s1ngl3_pr1m3_4nd_cl0se_prim3s_w3r3_w4st3_0f_c0mput3r_Cycl35}





