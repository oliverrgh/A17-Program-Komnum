# **Tugas Akhir Kelompok Komputasi Numerik**

# **Kelompok A17**
1. M. Fakhmi A. Y. (5025241027)
2. Agil Lukman H. (5025241037)
3. Jason Kumarkono (5025241105)
4. Indra Wahyu Tirtayasa (5025241108)
5. Nathanael Oliver A. Y. (5025241109)


# **Penjelasan Kode untuk Penyelesain pada nomor 31**

Berikut penjelasan mendetail tentang implementasi kode untuk menghitung turunan numerik menggunakan metode NGB:

## **1. Struktur Kode Utama**

```python
def calculate_backward_differences(y_values):
    # Fungsi menghitung selisih mundur
    pass

def round_two_decimal(num):
    # Fungsi pembulatan angka
    pass

def ng_backward_derivative(x_target, x0, h, y_values):
    # Fungsi utama perhitungan turunan NGB
    pass

# Data input
x0 = 15
h = 3
x_target = 16
y_backward = [634575, 184956, 32121, -186, -741]

# Eksekusi perhitungan
f_prime = ng_backward_derivative(x_target, x0, h, y_backward)
```

## **2. Fungsi calculate_backward_differences()**

```python
def calculate_backward_differences(y_values):
    n = len(y_values)
    differences = []
    temp = y_values.copy()
    
    for level in range(1, n):
        current_diff = []
        for i in range(len(temp)-1):
            diff = temp[i+1] - temp[i]
            current_diff.append(abs(diff))  # Mengambil nilai absolut
        differences.append(current_diff[0])  # Mengambil selisih pertama tiap level
        temp = current_diff
    
    return differences
```

**Penjelasan:**
- **Input:** Array nilai y dalam bentuk backward [y₀, y₋₁, y₋₂, ...]
- **Proses:**
  1. Membuat salinan array input
  2. Loop untuk setiap level selisih (1 sampai n-1)
  3. Hitung selisih antara elemen berturut-turut
  4. Ambil nilai absolut dari selisih
  5. Simpan selisih pertama setiap level
- **Output:** Array selisih mundur [Δ¹y₋₁, Δ²y₋₂, Δ³y₋₃, ...]

**Contoh Eksekusi:**
Untuk input [634575, 184956, 32121, -186, -741]:
- Level 1: [449619, 152835, 32307, 555]
- Level 2: [296784, 120528, 31752]
- Level 3: [176256, 88776]
- Level 4: [87480]
Output: [449619, 296784, 176256, 87480]

## **3. Fungsi round_two_decimal()**

```python
def round_two_decimal(num):
    return int(num * 100 + 0.5) / 100 if num >= 0 else int(num * 100 - 0.5) / 100
```

**Penjelasan:**
- Menggunakan teknik pembulatan standar
- Untuk bilangan positif: kalikan 100, tambah 0.5, konversi ke integer, bagi 100
- Untuk bilangan negatif: kalikan 100, kurangi 0.5, konversi ke integer, bagi 100
- Hasil selalu 2 angka desimal

**Contoh:**
- round_two_decimal(3.14159) → 3.14
- round_two_decimal(2.71828) → 2.72

## **4. Fungsi ng_backward_derivative()**

```python
def ng_backward_derivative(x_target, x0, h, y_values):
    # Langkah 1: Hitung selisih mundur
    differences = calculate_backward_differences(y_values)
    
    # Langkah 2: Hitung parameter s
    s = round_two_decimal((x_target - x0) / h)
    
    # Langkah 3: Hitung koefisien
    coeffs = [
        1.00,
        round_two_decimal((2*s + 1)/2),
        round_two_decimal((3*s**2 + 6*s + 2)/6),
        round_two_decimal((4*s**3 + 18*s**2 + 22*s + 6)/24)
    ]
    
    # Langkah 4: Hitung turunan
    derivative_terms = []
    total = 0.00
    
    for i in range(len(differences)):
        term = round_two_decimal(coeffs[i] * differences[i])
        derivative_terms.append(term)
        total = round_two_decimal(total + term)
    
    derivative = round_two_decimal(total / h)
    
    return derivative
```

**Penjelasan Langkah-langkah:**

1. **Menghitung Selisih Mundur**
   - Memanggil fungsi `calculate_backward_differences()`
   - Hasil: [Δ¹y₋₁, Δ²y₋₂, Δ³y₋₃, Δ⁴y₋₄]

2. **Menghitung Parameter s**
   - s = (x_target - x₀)/h
   - Dibulatkan ke 2 desimal

3. **Menghitung Koefisien NGB**
   - Koefisien 1: 1.00
   - Koefisien 2: (2s + 1)/2
   - Koefisien 3: (3s² + 6s + 2)/6
   - Koefisien 4: (4s³ + 18s² + 22s + 6)/24
   - Semua koefisien dibulatkan ke 2 desimal

4. **Menghitung Turunan**
   - Kalikan koefisien dengan selisih mundur
   - Jumlahkan semua term
   - Bagi dengan h untuk mendapatkan turunan akhir

## **5. Contoh Eksekusi Lengkap**

Untuk input:
- x_target = 16
- x0 = 15
- h = 3
- y_values = [634575, 184956, 32121, -186, -741]

**Proses Perhitungan:**

1. Selisih mundur:
   - Δ¹y₋₁ = 449619
   - Δ²y₋₂ = 296784
   - Δ³y₋₃ = 176256
   - Δ⁴y₋₄ = 87480

2. Parameter s:
   - s = (16-15)/3 ≈ 0.33

3. Koefisien:
   - coeffs = [1.00, 0.83, 0.72, 0.64]

4. Perhitungan term:
   - Term 1: 1.00 × 449619 = 449619.00
   - Term 2: 0.83 × 296784 ≈ 246330.72
   - Term 3: 0.72 × 176256 ≈ 126904.32
   - Term 4: 0.64 × 87480 ≈ 55987.20

5. Total:
   - 449619.00 + 246330.72 + 126904.32 + 55987.20 ≈ 878841.24

6. Turunan akhir:
   - f'(16) ≈ 878841.24 / 3 ≈ 292947.08

**Output yang dikembalikan:**
292947.08
