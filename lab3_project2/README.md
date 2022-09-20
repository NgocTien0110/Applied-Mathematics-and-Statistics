
## **Project 2 - IMAGE PROCESSING**

### Nội dung đồ án <a class="anchor" id="c21"></a>

Nhắc lại: Trong đồ án 1, bạn đã được giới thiệu rằng ảnh được lưu trữ dưới dạng ma trận các điểm ảnh. Mỗi điểm ảnh có thể là một giá trị (ảnh xám) hoặc một vector (ảnh màu).

Trong đồ án này, bạn được yêu cầu thực hiện các chức năng xử lý ảnh cơ bản sau:
    
1. Thay đổi độ sáng cho ảnh (1 điểm)

![img](https://i.imgur.com/XIaBAIv.jpg)

2. Thay đổi độ tương phản (1 điểm)

![img](https://i.imgur.com/4uxIHJD.jpg)

3. Lật ảnh (ngang - dọc) (2 điểm)

![img](https://i.imgur.com/VKjvVdc.jpg)

4. Chuyển đổi ảnh RGB thành ảnh xám (2 điểm)

![img](https://i.imgur.com/qJw14wS.jpg)

Tham khảo tại [đây](https://www.tutorialspoint.com/dip/grayscale_to_rgb_conversion.htm)

5. Chồng 2 ảnh cùng kích thước (1 điểm): chỉ làm trên ảnh xám

![img](https://i.imgur.com/no2NH1k.jpg)

6. Làm mờ ảnh (2 điểm)

![img](https://i.imgur.com/daY9Mnd.jpg)

Tham khảo tại [đây](https://en.wikipedia.org/wiki/Kernel_(image_processing)), phần Box blur hoặc Gaussian blur 3 $\times$ 3

7. Viết hàm main xử lý (1 điểm) với các yêu cầu sau:

- Cho phép người dùng nhập vào tên tập tin ảnh mỗi khi hàm main được thực thi.
- Cho phép người dùng lựa chọn chức năng xử lý ảnh (từ 1 đến 6, đối với chức năng 4 cho phép lựa chọn giữa lật ngang hoặc lật dọc). Lựa chọn 0 cho phép thực hiện tất cả chức năng với tên file đầu ra tương ứng với từng chức năng. Ví dụ:
    - Đầu vào: `cat.png`
    - Chức năng: Làm mờ
    - Đầu ra: `cat_blur.png`

Trong đồ án này, bạn <font style="color:red">**CHỈ ĐƯỢC PHÉP**</font> sử dụng các thư viện sau: `PIL`, `numpy`, `matplotlib`.

Cụ thể, nếu đề yêu cầu bạn viết ra chức năng đó, thì bạn phải thực sự viết ra chức năng đó chứ không phải gọi hàm có sẵn.

- Các bạn sử dụng `PIL` (`open(), save()` từ `Image`) để đọc và ghi; `Matplotlib` (`imshow()` từ `pyplot`) để hiển thị ảnh.

- Được phép sử dụng thư viện `NumPy` tùy ý.

Lưu ý: Để được **điểm tối đa** cho từng chức năng, thời gian thực thi của mỗi chức năng phải nằm trong khoảng thời gian chấp nhận được. Ví dụ với chức năng làm mờ (phức tạp nhất) có thời gian thực thi trên ảnh với kích thước $512 \times 512$ là dưới 20 giây.

#### Nâng cao - Không bắt buộc (Cộng 1 điểm vào điểm đồ án 2)

Sinh viên thực cắt nội dung ảnh theo khung được áp lên, với khung là hình như hình tròn và 2 hình ellip chéo nhau như các ảnh sau:

- Khung là hình tròn

![img](https://i.imgur.com/dH6OV4d.png)

- Khung là 2 hình ellip chéo nhau

![img](https://i.imgur.com/fPlYioC.png)

</br>