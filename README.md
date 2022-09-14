# <center>**Applied Mathematics and Statistics**<center>
</br>

## **Project 1 - COLOR COMPRESSION**

#### Giới thiệu

Một bức ảnh có thể lưu trữ dưới ma trận của các điểm ảnh. Có nhiều loại ảnh được sử dụng trong thực tế, ví dụ: ảnh xám, ảnh màu,... 

Đối với ảnh xám, một điểm ảnh sẽ là được biểu diễn bằng giá trị không âm. 

Ví dụ ta có thể dùng ma trận này:

$$\begin{bmatrix}
255 & 0 & 0  & 0  & 255 \\ 
255 & 0 & 255 & 0 & 255\\ 
255 & 0 & 255 & 0 & 255\\ 
255 & 0 & 255 & 0 & 255\\ 
255 & 0 & 0  & 0  & 255
\end{bmatrix}$$

có thể biểu diễn cho ảnh xám có nội dung như sau:
```python
import matplotlib.pyplot as plt
plt.imshow(np.array([[255, 0, 0, 0, 255], [255, 0, 255, 0, 255], [255, 0, 255, 0, 255], 
                     [255, 0, 255, 0, 255], [255, 0, 0, 0, 255]]), cmap='gray', vmin=0, vmax=255);
```

Ảnh màu được sử dụng phổ biến là ảnh RGB, trong đó, mỗi điểm ảnh sẽ lưu trữ 3 thông tin kênh màu (mỗi kênh màu 1 byte) là: R (red - đỏ), G (green - xanh lá), B (blue - xanh dương). Ta có thể sử dụng ma trận:
```
                    [[[255, 255, 255], [255, 0, 0], [255, 0, 0], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 255, 255], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 255, 255], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 255, 255], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 0, 0], [255, 0, 0], [255, 255, 255]]]
```
để biểu diễn cho ảnh màu có nội dung sau:

```python
plt.imshow(np.array([[[255, 255, 255], [255, 0, 0], [255, 0, 0], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 255, 255], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 255, 255], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 255, 255], [255, 0, 0], [255, 255, 255]],
                     [[255, 255, 255], [255, 0, 0], [255, 0, 0], [255, 0, 0], [255, 255, 255]]]));
```

Như vậy, số màu trong ảnh RGB có thể là $256^3 \approx 1.7 \times 10^7$. Với số lượng màu khá lớn, khi lưu trữ ảnh có thể sẽ tốn chi phí lưu trữ. Do đó bài toán đặt ra là giảm số lượng màu để biểu diễn ảnh sao cho nội dung ảnh được bảo toàn nhất có thể.

Cho ảnh như sau:

![img](https://scontent.fhan4-2.fna.fbcdn.net/v/t1.6435-9/108021534_2792519034314628_43786322214829236_n.jpg?_nc_cat=111&ccb=1-7&_nc_sid=730e14&_nc_ohc=u7A7KXOiHU8AX8tU79-&tn=vv22AQwGHZ3TiJCI&_nc_ht=scontent.fhan4-2.fna&oh=00_AT9DjF6wuEeMVU2JmKk4vhjulF65hj2ltIftCLLWA_zhQg&oe=62B9F090)

Trong ví dụ trên, số lượng màu cho ảnh ban đầu là 439 màu. Sau khi giảm số lượng màu xuống còn 5, ảnh không còn được chi tiết nhưng cơ bản vẫn bảo toàn nội dung của ảnh ban đầu.

Để thực hiện giảm số lượng màu, ta cần tìm ra các đại diện có thể thay thế cho một nhóm màu. Cụ thể trong trường hợp ảnh RGB, ta cần thực hiện gom nhóm các pixel ($\mathbb{R}^3$) và chọn ra đại diện cho từng nhóm. Như vậy, bài toán trên trở thành gom nhóm các vec-tơ.

#### Yêu cầu

Trong đồ án này, bạn được yêu cầu cài đặt chương trình giảm số lượng màu cho ảnh sử dụng thuật toán [K-Means](https://en.wikipedia.org/wiki/K-means_clustering).

Các thư viện được phép sử dụng là: `NumPy` (tính toán ma trận), `PIL` (đọc, ghi ảnh), `matplotlib` (hiển thị ảnh).

Một số gợi ý:
- Đọc ảnh: `PIL.Image.open(...)`
- Hiển thị ảnh: `matplotlib.pyplot.imshow(...)`
- Thay đổi shape cho `np.ndarray`: `np.reshape(...)`
- Khai báo hàm gợi ý cho thuật toán K-Means:
```python
def kmeans(img_1d, k_clusters, max_iter, init_centroids='random'):
    '''
    K-Means algorithm
    
    Inputs:
        img_1d : np.ndarray with shape=(height * width, num_channels)
            Original image in 1d array
        
        k_clusters : int
            Number of clusters
            
        max_iter : int
            Max iterator
            
        init_cluster : str
            The way which use to init centroids
            'random' --> centroid has `c` channels, with `c` is initial random in [0,255]
            'in_pixels' --> centroid is a random pixels of original image
            
    Outputs:
        centroids : np.ndarray with shape=(k_clusters, num_channels)
            Store color centroids
            
        labels : np.ndarray with shape=(height * width, )
            Store label for pixels (cluster's index on which the pixel belongs)
    
    '''
    
    ### YOUR CODE HERE
```

<font style="color:red">*Lưu ý: Không sử dụng K-Means đã được cài đặt sẵn trong các thư viện trong bài nộp. Bạn có thể sử dụng K-Means trong `scikit-learn` để kiểm tra.* </font>

**Sinh viên cần viết chương trình `main` cho phép**:
- Người dùng nhập vào tên tập tin ảnh mỗi lần chương trình thực thi (gợi ý sử dụng `input()` trong Python)
- Lựa chọn định dạng lưu ảnh đầu ra gồm: `png`, `jpg`, `pdf`

</br>

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

## **Project 3 - Linear Regression**

### Mô tả dữ liệu <a class="anchor" id="c11"></a>

#### Dữ liệu Tuổi thọ trung bình (Life expectancy)

Dữ liệu tuổi thọ trung bình được thu thập từ tổ chức WHO và trang web United Nations từ năm 2000 đến 2015 trên tất cả quốc gia.

Bộ dữ liệu có 2938 dòng và 22 cột. Ý nghĩa và kiểu dữ liệu của từng cột được thể hiện ở bảng sau:

| STT | Tên cột                         | Ý nghĩa                                                                                   | Kiểu dữ liệu |
|:---:|:---------------------------------|:-------------------------------------------------------------------------------------------|:------------:|
|  1  | Country                         | Tên quốc gia                                                                              |    String    |
|  2  | Year                            | Năm                                                                                       |    Integer   |
|  3  | Status                          | Đất nước phát triển hay đang phát triển                                                   |    String    |
|  4  | Life expectancy                 | Tuổi thọ trung bình                                                                       |    Decimal   |
|  5  | Adult mortality                 | Tỉ lệ tử vong ở người trưởng thành trên 1000 dân số (độ tuổi từ 15 đến 60)                |    Decimal   |
|  6  | Infant deaths                   | Số trẻ sơ sinh tử vong trên 1000 dân số                                                   |    Decimal   |
|  7  | Alcohol                         | Mức tiêu thụ rượu bình quân đầu người (từ 15 tuổi) - tính trên lít rượu nguyên chất       |    Decimal   |
|  8  | percentage expenditure          | Chi tiêu cho y tế tính theo phần trăm GDP (%)                                             |    Decimal   |
|  9  | Hepatitis B                     | Tỉ lệ tiêm chủng viêm gan B ở trẻ 1 tuổi (%)                                              |    Decimal   |
|  10 | Measles                         | Số ca mắc bệnh sởi được báo cáo tính trên 1000 dân số                                     |    Integer   |
|  11 | BMI                             | Chỉ số khối lượng cơ thể (BMI) trung bình của toàn bộ dân số                              |    Decimal   |
|  12 | under-five deaths               | Số lượng người tử vong dưới 5 tuổi trên 1000 dân số                                          |    Integer   |
|  13 | Polio                           | Tỉ lệ tiêm chủng bại liệt (Pol3) ở trẻ 1 tuổi (%)                                         |    Decimal   |
|  14 | Total expenditure               | Chi tiêu chung cho y tế tính tính theo phần trăm tổng chi tiêu của chính phủ              |    Decimal   |
|  15 | Diphtheria                      | Tỉ lệ tiêm chủng giải độc uốn ván và ho gà (DTP3) ở trẻ 1 tuổi (%)                        |    Decimal   |
|  16 | HIV/AIDS                        | Tỉ lệ tử vong trên 1000 trẻ nhiễm HIV/AIDS (từ 0 đến 4 tuổi)                              |    Decimal   |
|  17 | GPD                             | Tổng sản phẩm quốc nội bình quân đầu người (đơn vị USD)                                   |    Decimal   |
|  18 | Population                      | Dân số quốc gia                                                                           |    Decimal   |
|  19 | thinness 1-19 years             | Tỉ lệ gầy ốm ở trẻ em và thanh thiếu niên từ 10-19 tuổi (%)                               |    Decimal   |
|  20 | thinness 5-9 years              | Tỉ lệ gầy ốm ở trẻ em từ 5-9 tuổi (%)                                                     |    Decimal   |
|  21 | Income composition of resources | Chỉ số phát triển con người (HDI) tính theo thành phần thu nhập tài nguyên (thuộc [0, 1]) |    Decimal   |
|  22 | Schooling                       | Số năm đi học                                                                             |    Decimal   |

Sinh viên đọc thêm về dữ liệu tại: [Life Expectancy (WHO)](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who).

#### Dữ liệu sử dụng cho đồ án

Trong đồ án này, dữ liệu trên đã được thực hiện các bước tiền xử lý sau:
1. Loại bỏ các dòng dữ liệu không có đầy đủ thông tin (có giá trị NaN trong dòng)
2. Chỉ chọn các dòng liên quan đến top 95 quốc gia có dân số đông nhất
3. Chuẩn hóa và đổi tên một số đặc trưng: `thinness 1-19 years` $\to$ `Thinness age 10-19`, `thinness 5-9 years` $\to$ `Thinness age 5-9`
4. Loại bỏ 2 cột có giá trị là chuỗi:  `Country`, `Status`
5. Dựa trên độ đo tương quan, loại bỏ 9 cột ít liên quan đến giá trị mục tiêu (`Life expectancy`) nhất: `Population`, `Measles`, `Year`, `infant deaths`, `Total expenditure`, `under-five deaths`, `Hepatitis B`, `percentage expenditure`, `Alcohol`

Sau quá trình tiền xử lý, dữ liệu mới có:
- 1180 dòng dữ liệu
- 11 cột dữ liệu gồm:
    - 1 giá trị mục tiêu ($y$): `Life expectancy`
    - 10 đặc trưng giải thích $(X)$ (đặc trưng giúp tìm giá trị mục tiêu) gồm: `Adult Mortality`, `BMI`, `Polio`, `Diphtheria`, `HIV/AIDS`, `GDP`, `Thinness age 10-19`, `Thinness age 5-9`, `Income composition of resources`, `Schooling`, `Life expectancy`

Sinh viên được cung cấp 2 tập tin:
- `train.csv`: Chứa 1085 mẫu dùng để huấn luyện mô hình
- `test.csv`: Chứa 95 mẫu dùng để kiểm tra mô hình

Đoạn mã nguồn sau để thể hiện 5 mẫu dữ liệu đầu tiên trong tập `train.csv`.

### Yêu cầu <a class="anchor" id="c12"></a>

Trong đồ án này, sinh viên được yêu cầu thực hiện:

1. Xây dựng mô hình *dự đoán tuổi thọ trung bình* **sử dụng hồi quy tuyến tính** (7 điểm)

- Yêu cầu 1a: Sử dụng toàn bộ 10 đặc trưng đề bài cung cấp (2 điểm)
	- Huấn luyện 1 lần duy nhất cho 10 đặc trưng trên toàn bộ tập huấn luyện (`train.csv`)
	- Thể hiện công thức cho mô hình hồi quy (tính $y$ theo 10 đặc trưng trong $X$)
	- Báo cáo **1 kết quả trên tập kiểm tra (`test.csv`)** cho mô hình vừa huấn luyện được
    
- Yêu cầu 1b: Xây dựng mô hình sử dụng duy nhất 1 đặc trưng, tìm mô hình cho kết quả tốt nhất (2 điểm)
	- Thử nghiệm trên toàn bộ (10) đặc trưng đề bài cung cấp
	- Yêu cầu **sử dụng phương pháp 5-fold Cross Validation** để tìm ra đặc trưng tốt nhất
	- Báo cáo **10 kết quả tương ứng cho 10 mô hình** từ 5-fold Cross Validation (lấy trung bình)
	
	<center>

	| STT | Mô hình với 1 đặc trưng | RMSE |
	|:---:|:-----------------------:|:----:|
	|  1  | Adult Mortality         |      |
	|  2  | BMI                     |      |
	|     | ...                     |      |
	|  10 | Schooling               |      |

	</center>

	- Thể hiện công thức cho mô hình hồi quy theo đặc trưng tốt nhất (tính $y$ theo đặc trưng tốt nhất tìm được)
    - Báo cáo **1 kết quả trên tập kiểm tra (`test.csv`)** cho mô hình tốt nhất tìm được
	
- Yêu cầu 1c: Sinh viên tự xây dựng mô hình, tìm mô hình cho kết quả tốt nhất (3 điểm)
	- Xây dựng `m` mô hình khác nhau (tối thiểu 3), đồng thời khác mô hình ở 1a và 1b
		- Mô hình có thể là sự kết hợp của 2 hoặc nhiều đặc trưng
		- Mô hình có thể sử dụng đặc trưng đã được chuẩn hóa hoặc biến đổi (bình phương, lập phương...)
		- Mô hình có thể sử dụng đặc trưng được tạo ra từ 2 hoặc nhiều đặc trưng khác nhau (cộng 2 đặc trưng, nhân 2 đặc trưng...)
		- ...
	- Yêu cầu **sử dụng phương pháp 5-fold Cross Validation** để tìm ra mô hình tốt nhất
	- Báo cáo **`m` kết quả tương ứng cho `m` mô hình** từ 5-fold Cross Validation (lấy trung bình)

	<center>

	| STT |           Mô hình          | RMSE |
	|:---:|:--------------------------:|:----:|
	|  1  | Sử dụng 2 đặc trưng (a, b) |      |
	| ... | ...                        |      |
	|  m  | ...                        |      |

	</center>

	- Thể hiện công thức cho mô hình hồi quy tốt nhất mà sinh viên tìm được
	- Báo cáo **1 kết quả trên tập kiểm tra (`test.csv`)** cho mô hình tốt nhất tìm được

- <ins>Lưu ý:</ins>
    - Tập `test.csv` chỉ được sử dụng 3 lần như được đề cập bên trên
    - Kết quả báo cáo là độ đo [RMSE](https://www.sciencedirect.com/topics/engineering/root-mean-squared-error)
    

2. Báo cáo về kết quả, đánh giá và nhận xét các mô hình đã xây dựng (3 điểm)
    - Báo cáo cần có thông tin cá nhân (họ và tên, MSSV), số trang và tài liệu tham khảo (cần chỉ định cụ thể tài liệu được sử dụng cho phần nào trong bài làm)
    - Liệt kê TẤT CẢ thư viện đã sử dụng và lý do sử dụng chúng
	- Liệt kê TẤT CẢ hàm đã sử dụng và mô tả các hàm đó
    - Báo cáo và nhận xét kết quả từ TOÀN BỘ các mô hình xây dựng được (có $1 + (10 + 1) + (m + 1)$ kết quả)
    - Với yêu cầu 1b và 1c: giải thích hoặc nêu giả thuyết cho mô hình đạt kết quả tốt nhất
	- Với yêu cầu 1c: giải thích lý do vì sao lại trích chọn/thiết kế các đặc trưng cho `m` mô hình




