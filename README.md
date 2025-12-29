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
``df = df.dropna(subset=["father_profession"])``
#### Kiểm tra và loại bỏ các dòng trùng lặp
``df.info()
df.head()``
#### Kiểm tra thông tin và hiển thị dữ liệu sau xử lý
``df.info()
df.head()``
## Chuyển đổi cột cân nặng sang kiểu số
#### Chuyển cột weight sang dạng số, ép các giá trị không hợp lệ thành NaN
``df["weight"] = pd.to_numeric(df["weight"], errors="coerce")``
#### Xác định các biến định lượng dùng cho phân tích
``numeric_cols = [
    "GPA",
    "calories_day", "calories_chicken", "calories_scone",
    "tortilla_calories", "turkey_calories", "waffle_calories",
    "fruit_day", "veggies_day",
    "exercise",
    "healthy_feeling", "healthy_meal",
    "income",
    "weight"
]``
#### Tính toán thống kê mô tả
##### Tính các chỉ tiêu: giá trị trung bình, độ lệch chuẩn, nhỏ nhất và lớn nhất
``summary = df[numeric_cols].describe().loc[["mean", "std", "min", "max"]]``
##### Xuất bảng thống kê ra file CSV
``summary.to_csv("statistics_summary.csv")``
##### Hiển thị kết quả thống kê để kiểm tra
``print(summary)``
## Import thư viện trực quan hóa dữ liệu
``import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns``
## Phân phối điểm GPA (Histogram)
``plt.figure(figsize=(6, 3))
sns.histplot(df["GPA"], bins=20, kde=True, color="skyblue")
plt.title("Phân phối GPA")
plt.xlabel("GPA")
plt.ylabel("Tần suất")
plt.show()``
## Phát hiện ngoại lai trong lượng calo tiêu thụ mỗi ngày (Boxplot)
``plt.figure(figsize=(5, 4))
sns.boxplot(x=df["calories_day"], color="lightgreen")
plt.title("Boxplot calories_day (phát hiện outlier)")
plt.xlabel("Calories per day")
plt.show()``
## Phân tích tương quan giữa các biến chính (Heatmap)
``corr_cols = ["GPA", "exercise", "calories_day", "healthy_feeling"]
corr_matrix = df[corr_cols].corr()

plt.figure(figsize=(5, 4))
sns.heatmap(corr_matrix, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Heatmap tương quan")
plt.show()``
## So sánh GPA trung bình theo giới tính
``plt.figure(figsize=(5, 4))
sns.barplot(
    x="Gender", y="GPA", data=df,
    estimator="mean", errorbar=None,
    hue="Gender", legend=False, palette="Set2"
)
plt.title("GPA trung bình theo giới tính")
plt.xlabel("Giới tính (1 = Nam, 2 = Nữ)")
plt.ylabel("GPA trung bình")
plt.show()
## So sánh GPA trung bình theo thói quen ăn sáng
plt.figure(figsize=(5, 4))
sns.barplot(
    x="breakfast", y="GPA", data=df,
    estimator="mean", errorbar=None,
    hue="breakfast", legend=False, palette="Set3"
)
plt.title("GPA trung bình theo thói quen ăn sáng")
plt.xlabel("Ăn sáng (1 = Có, 2 = Không)")
plt.ylabel("GPA trung bình")
plt.show()``
## Dự đoán self_perception_weight hoặc healthy_feeling Sử dụng các biến đầu vào: diet_current, exercise, calories_day
``import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score``
#### Chuyển đổi cột diet_current sang số (Label Encoding)
``le = LabelEncoder()
df["diet_current_encoded"] = le.fit_transform(df["diet_current"].astype(str))``
#### Chọn biến đầu vào và biến mục tiêu
``X = df[["diet_current_encoded", "exercise", "calories_day"]].dropna()
y = df["healthy_feeling"].loc[X.index]   # hoặc thay bằng self_perception_weight``
#### Chia dữ liệu train/test
``X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)``
#### Mô hình hồi quy tuyến tính
``model = LinearRegression()
model.fit(X_train, y_train)``
# Dự đoán
``y_pred = model.predict(X_test)``
# Đánh giá
``print("MSE:", mean_squared_error(y_test, y_pred))
print("R2:", r2_score(y_test, y_pred))``
## Dự đoán calories_day từ thói quen ăn uống và sở thích thực phẩm. dùng các biến: breakfast, eating_out, fav_cuisine, fav_food
#### Chuyển đổi các biến định tính sang số
``df["fav_cuisine_encoded"] = le.fit_transform(df["fav_cuisine"].astype(str))
df["fav_food_encoded"] = le.fit_transform(df["fav_food"].astype(str))``
#### Chọn biến đầu vào và biến mục tiêu
``X2 = df[["breakfast", "eating_out", "fav_cuisine_encoded", "fav_food_encoded"]].dropna()
y2 = df["calories_day"].loc[X2.index]``
#### Chia dữ liệu train/test
``X2_train, X2_test, y2_train, y2_test = train_test_split(X2, y2, test_size=0.2, random_state=42)``
#### Mô hình hồi quy tuyến tính
``model2 = LinearRegression()``
``model2.fit(X2_train, y2_train)``
#### Dự đoán
``y2_pred = model2.predict(X2_test)``
#### Đánh giá
``print("MSE:", mean_squared_error(y2_test, y2_pred))
print("R2:", r2_score(y2_test, y2_pred))``
