# PhishingDetection

![Python](https://img.shields.io/badge/python-3.7%2B-blue.svg)
![Django](https://img.shields.io/badge/Django-2.1.7-green.svg)
![License](https://img.shields.io/badge/license-Educational-lightgrey.svg)
![Status](https://img.shields.io/badge/status-active-success.svg)


 Project OUTPUT's

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/613983bb-0208-47e6-87d1-bfa82f9a0a60" />

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/9a4e5428-c412-4ed1-8352-ea648fbbadda" />


<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b312a993-6a94-4b62-8fad-c22b4776dc52" />


<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/1943a552-1d60-4167-aa07-0425b2efaf01" />



<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/9f4be2fb-208e-4619-a377-69641599fdc6" />


<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/efdc4ae2-67a7-4054-966c-c5ed87570224" />






 
 📌 Overview
PhishingDetection is a Django-based web application designed to detect and analyze phishing attempts using machine learning.  
It provides a web interface where users can test URLs against a trained phishing detection model.

 📂 Project Structure
PhishingDetection/
│
├── PhishingDetection/          # Main project folder
│   ├── settings.py             # Django settings
│   ├── wsgi.py                 # WSGI entry point
│   ├── urls.py                 # URL routing
│
├── PhishingDetectionApp/       # Core application logic
│   ├── models/                 # ML models (saved here)
│   ├── templates/              # HTML templates
│   ├── static/                 # CSS/JS/Images
│   ├── data/                   # Dataset storage
│
├── db.sqlite3                  # SQLite database (default)
├── run.bat                     # Windows script to start the server
├── train\_model.py              # Model training script

⚙️ Requirements
- Python 3.7+  
- Django 2.1.7  
- scikit-learn  
- pandas  
- numpy  

Install dependencies:
```bash
pip install -r requirements.txt
````

If `requirements.txt` is missing, install manually:
bash
pip install django==2.1.7 scikit-learn pandas numpy


---

🚀 Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/PhishingDetection.git
   cd PhishingDetection
   ```
2. Create and activate a virtual environment:

   ```bash
   python -m venv venv
   venv\Scripts\activate   # On Windows
   source venv/bin/activate  # On Linux/Mac
   ```
3. Install dependencies (see above).

---

 ▶️ Running the Application

 1. Using the batch script (Windows)

The provided `run.bat` file contains:

bat
python manage.py runserver

Double-click `run.bat` or run in **Command Prompt**:
bash
run.bat

 2. Using Django commands directly

```bash
python manage.py runserver
```

Server will start at:

```
http://127.0.0.1:8000/
```

---

📊 Dataset Usage

* Place your phishing dataset (CSV format recommended) inside the `PhishingDetectionApp/data/` folder.
* The dataset should contain labeled URLs (e.g., `url`, `label`) where:

  * `label = 1` → Phishing
  * `label = 0` → Legitimate

Example:

```csv
url,label
http://example.com,0
http://malicious-site.com,1
```

---

🧠 Model Training

You can train the phishing detection model using the provided `train_model.py`.

 Script: `train_model.py`

```python
import pandas as pd
import pickle
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics import accuracy_score
import os

# Load dataset
DATA_PATH = "PhishingDetectionApp/data/phishing.csv"
MODEL_PATH = "PhishingDetectionApp/models/phishing_model.pkl"

print("[INFO] Loading dataset...")
data = pd.read_csv(DATA_PATH)

# Extract features and labels
urls = data['url']
labels = data['label']

# Convert URLs into features using Bag-of-Words
print("[INFO] Extracting features...")
vectorizer = CountVectorizer()
X = vectorizer.fit_transform(urls)
y = labels

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train model
print("[INFO] Training model...")
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Evaluate model
y_pred = model.predict(X_test)
print(f"[INFO] Accuracy: {accuracy_score(y_test, y_pred):.2f}")

# Save model + vectorizer
os.makedirs(os.path.dirname(MODEL_PATH), exist_ok=True)
pickle.dump((model, vectorizer), open(MODEL_PATH, "wb"))
print(f"[INFO] Model saved at {MODEL_PATH}")
```

 Run Training

```bash
python train_model.py
```

This will:

1. Load dataset from `PhishingDetectionApp/data/phishing.csv`
2. Train a RandomForest model
3. Save the trained model + vectorizer as `PhishingDetectionApp/models/phishing_model.pkl`

---

🔍 Using Model in Django

Example in `views.py`:

```python
import pickle
import os

MODEL_PATH = os.path.join("PhishingDetectionApp", "models", "phishing_model.pkl")
model, vectorizer = pickle.load(open(MODEL_PATH, "rb"))

def check_url(request):
    url = request.POST.get("url")
    features = vectorizer.transform([url])
    prediction = model.predict(features)[0]
    result = "Phishing" if prediction == 1 else "Legitimate"
    return render(request, "result.html", {"url": url, "result": result})
```

---

 🌍 Deployment

The project includes `wsgi.py` for deployment with WSGI-compatible servers like **Gunicorn**, **uWSGI**, or **Apache mod\_wsgi**.

---

📜 License

![License](https://img.shields.io/badge/license-Educational-lightgrey.svg)
This project is provided for **educational and research purposes only**.

```

---

✨ Now your GitHub repo will look **clean and professional** with badges at the top.  

Do you also want me to **generate the `requirements.txt` file** for you right now, so you can just commit it along with the README?
```
