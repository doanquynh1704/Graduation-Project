# Graduation-Project
kKhảo sát về thói quen ăn uống, sở thích thực phẩm, nhận thức về sức khỏe và lối sống của một nhóm sinh viên ở một trường ĐH nước ngoài. Bộ dữ liệu được sử dụng có nguồn từ Kaggle.
# 1. Giới thiệu về bộ dữ liệu
- **Chủ đề**: Lựa chọn và sở thích ăn uống của sinh viên đại học.  
- **Mô tả**: Bộ dữ liệu này bao gồm thông tin về lựa chọn ăn uống, dinh dưỡng, sở thích, món ăn yêu thích thời thơ ấu và các thông tin khác từ sinh viên đại học.  
- **Số lượng phản hồi**: 126 phản hồi từ sinh viên.  
- **Link dữ liệu**: [Xem tại đây](https://docs.google.com/spreadsheets/d/1fg6c4NBmZdGw5XUZopaH0eQFm2IbbmIfaIs6Sbwvxfk/edit?usp=sharing)

---

# 2. Nội dung dữ liệu

###  Thông tin cá nhân
- `GPA`
- `Gender`
- `income`
- `grade_level`
- `employment`
- `marital_status`

###  Thói quen ăn uống
- `breakfast`
- `eating_out`
- `meals_dinner_friend`
- `parents_cook`
- `pay_meal_out`

###  Sở thích thực phẩm
- `fav_food`
- `fav_cuisine`
- `comfort_food`
- `ideal_diet`
- `healthy_meal`

###  Nhận thức sức khỏe
- `healthy_feeling`
- `exercise`
- `vitamins`
- `self_perception_weight`

###  Lượng calo tiêu thụ
- `calories_day`
- `calories_chicken`
- `calories_scone`
- `waffle_calories`
- `turkey_calories`
- `tortilla_calories`

###  Yếu tố tâm lý và cảm xúc
- `comfort_food_reasons`
- `life_rewarding`
- `eating_changes`
# 3. Xử lý dữ liệu ban đầu
- **Kiểm tra các giá trị thiếu (NaN) và xử lý chúng:**
  - Cột `GPA` NaN: **loại bỏ**
  - `calories_day`: điền số **2**
  - `comfort_food_reasons`: **bỏ**
  - `comfort_food_reasons_coded`: điền số **8** và **4** vào các ô trống
  - `cook`: điền số **5**
  - `cuisine`: điền **4, 5, 6** lần lượt vào ô trống
  - `drink`: điền số **1**
  - `employment`: điền số **1**
  - `exercise`: điền số **3**
  - `father_education`: điền số **1**
  - `father_profession` NaN: **loại bỏ**
  - `fav_food`: điền số **2**
  - `marital_status`: điền số **1**
  - `mother_education`: điền số **1**
  - `on_off_campus`: điền số **4**
  - `persian_food`: điền số **4**
  - `self_perception_weight`: điền số **6**
  - `soup`: điền số **2**
  - `sports`: điền số **2**
  - `tortilla_calories`: điền số **580**
  - `weight`: điền `"I'm not answering this."`
  - `calories_scone`: điền số **315**

- **Kiểm tra dữ liệu trùng lặp và loại bỏ**
# 4. Sử dụng Power BI vẽ các biểu đồ để thể hiện thông tin tổng quan (Overview)

- **Card visual**: Hiển thị số lượng sinh viên tham gia khảo sát (**119**)
- **Pie chart**: Phân bố giới tính (`Gender`)
- **Donut chart**: Tỷ lệ sinh viên ăn sáng (`breakfast`)
- **Clustered bar chart**: Fav cuisine (`fav_cuisine`) theo giới tính
- **Clustered bar chart**: So sánh số lần ăn ngoài (`eating_out`) theo thu nhập (`income`)
- **Scatter chart**: Thói quen tập thể dục theo `GPA`

# 5. Sử dụng python để xử lý dữ liệu
## Cài đặt và import thư viện
```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

## Khai báo đường dẫn googlesheet
url = "https://docs.google.com/spreadsheets/d/1N21Vl_Twr9JLNMb-X0J9A-tj-QYJl7p5kp8SyldPs-A/export?format=csv&gid=1890386386"
## Đọc dữ liệu từ Google Sheets vào DataFrame
df = pd.read_csv(url)

## Hiển thị 5 dòng dữ liệu đầu tiên
df.head()

## Làm sạch dữ liệu GPA
#### Loại bỏ các dòng có giá trị GPA bị thiếu
df = df.dropna(subset=["GPA"])
#### Điền giá trị mặc định cho các cột còn thiếu
fill_values = {
    "calories_day": 2,
    "comfort_food_reasons": "bỏ",   # Có thể để NaN nếu muốn xử lý sau
    "comfort_food_reasons_coded": 8,
    "cook": 5,
    "cuisine": 4,
    "drink": 1,
    "employment": 1,
    "exercise": 3,
    "father_education": 1,
    "fav_food": 2,
    "marital_status": 1,
    "mother_education": 1,
    "on_off_campus": 4,
    "persian_food": 4,
    "self_perception_weight": 6,
    "soup": 2,
    "sports": 2,
    "tortilla_calories": 580,
    "weight": "I'm not answering this.",
    "calories_scone": 315
}

df = df.fillna(value=fill_values)
#### Loại bỏ dữ liệu thiếu ở nghề nghiệp của cha
df = df.dropna(subset=["father_profession"])
#### Kiểm tra và loại bỏ các dòng trùng lặp
df.info()
df.head()
#### Kiểm tra thông tin và hiển thị dữ liệu sau xử lý
df.info()
df.head()
