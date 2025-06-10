
# 📘 API Prediksi Rating Tempat Wisata

API ini digunakan untuk memprediksi **rating** dari tempat wisata berdasarkan **lokasi, harga, dan kategori**. Model ini menggunakan **TensorFlow Keras**, serta preprocessing melalui `StringLookup` dan `StandardScaler` yang telah disimpan dalam format pickle.

---

## 🌐 Endpoint

**Base URL**  
`http://127.0.0.1:5000/predict`

**Method**  
`POST`

---

## 📋 Headers

```http
Content-Type: application/json
```

---

## 📦 Request Body (JSON)

```json
{
  "Place_Name": "Candi Borobudur",
  "City": "Yogyakarta",
  "Category": "Budaya",
  "Price": 50000,
  "Lat": -7.6079,
  "Long": 110.2038
}
```

### Penjelasan Field:

| Field         | Tipe     | Wajib | Deskripsi                                  |
|---------------|----------|-------|--------------------------------------------|
| `Place_Name`  | string   | Tidak | Nama tempat (untuk ditampilkan saja)       |
| `City`        | string   | Ya    | Kota tempat wisata berada                  |
| `Category`    | string   | Ya    | Kategori tempat wisata (misal: Budaya)     |
| `Price`       | float    | Ya    | Harga tiket masuk tempat wisata (dalam IDR)|
| `Lat`         | float    | Ya    | Koordinat latitude                         |
| `Long`        | float    | Ya    | Koordinat longitude                        |

---

## ✅ Contoh Request (Python)

```python
import requests

url = "http://localhost:5000/predict"
payload = {
    "Place_Name": "Pantai Parangtritis",
    "City": "Yogyakarta",
    "Category": "Nature",
    "Price": 10000,
    "Lat": -8.0206,
    "Long": 110.3252
}
headers = {"Content-Type": "application/json"}

response = requests.post(url, json=payload, headers=headers)
print(response.json())
```

---

## 📤 Contoh Response

```json
{
  "place_name": "Pantai Parangtritis",
  "predicted_rating": 3.9780099391937256
}
```

---

## ❌ Contoh Error Response

```json
{
  "error": "'City'"
}
```

> Error ini muncul jika parameter wajib tidak disertakan dalam request.

---

## 🛠️ Teknologi & File yang Digunakan

| Komponen               | File                         | Deskripsi                              |
|------------------------|------------------------------|----------------------------------------|
| Model ML               | `model_tempat.h5`            | Model Keras untuk prediksi rating      |
| Scaler fitur numerik   | `scaler_tempat.pkl`          | StandardScaler untuk fitur numerik     |
| Lookup kategori        | `lookup_tempat_category.pkl` | One-hot encoder untuk kolom `Category` |
| Lookup kota            | `lookup_tempat_city.pkl`     | One-hot encoder untuk kolom `City`     |
| API Service            | `app.py`                     | Script Flask API                       |

---

## 🔁 Alur Prediksi

1. Ambil data dari request body.
2. Encode `City` dan `Category` menggunakan `StringLookup`.
3. Normalisasi `Price`, `Lat`, dan `Long` menggunakan `StandardScaler`.
4. Gabungkan semua fitur menjadi satu tensor.
5. Lakukan prediksi rating menggunakan model Keras.
6. Kembalikan hasil prediksi dalam format JSON.
