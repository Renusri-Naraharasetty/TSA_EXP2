# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 02.05.26
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
# EXP 2


import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("/content/final_website_stats.csv")

df['timestamp'] = pd.to_datetime(df['timestamp'], format='%d-%m-%Y')

df.head()
df.tail()

years = df['timestamp'].dt.year.tolist()
page_views = df['page_views'].tolist()

X = [i - years[len(years) // 2] for i in years]

# ---------------- LINEAR TREND ---------------- #
x2 = [i**2 for i in X]
xy = [i*j for i, j in zip(X, page_views)]
n = len(years)

b = (n * sum(xy) - sum(page_views) * sum(X)) / (n * sum(x2) - (sum(X)**2))
a = (sum(page_views) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]

# ---------------- POLYNOMIAL TREND ---------------- #
x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i*j for i, j in zip(x2, page_views)]

coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(page_views), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * (X[i]**2)
    for i in range(n)
]

# ---------------- EQUATIONS ---------------- #
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

df['Linear Trend'] = linear_trend
df['Polynomial Trend'] = poly_trend

# ---------------- LINEAR TREND GRAPH ---------------- #
plt.figure(figsize=(10, 5))

plt.plot(years, page_views, 'bo-', label="Real Page Views")
plt.plot(years, linear_trend, 'k--', label="Linear Trend")

plt.title("Website Page Views with Linear Trend")
plt.xlabel("Year")
plt.ylabel("Page Views")

plt.legend()
plt.grid(True)

plt.show()

# ---------------- POLYNOMIAL TREND GRAPH ---------------- #
plt.figure(figsize=(10, 5))

plt.plot(years, page_views, 'bo-', label="Real Page Views")
plt.plot(years, poly_trend, 'k-', label="Polynomial Trend (Quadratic)")

plt.title("Website Page Views with Polynomial Trend")
plt.xlabel("Year")
plt.ylabel("Page Views")

plt.legend()
plt.grid(True)

plt.show()
```

### OUTPUT
<img width="525" height="51" alt="image" src="https://github.com/user-attachments/assets/c225f5ff-0611-4b61-a0e5-9d6fe286c356" />

A - LINEAR TREND ESTIMATION
<img width="1070" height="578" alt="image" src="https://github.com/user-attachments/assets/24e69b5b-5e20-42e7-bc72-6fbc5e4b9835" />

B- POLYNOMIAL TREND ESTIMATION
<img width="1072" height="587" alt="image" src="https://github.com/user-attachments/assets/c042d4a8-455f-4acf-a5b7-fe32064196f0" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
