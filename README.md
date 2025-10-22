
#  FraudNix

**FraudNix** is a complete full-stack demo for simulating financial transactions and classifying them as **Approved**, **Flagged**, or **Fraudulent** using a live-trained machine learning model.  

The system includes:
- A **Python/Flask backend**
- A **relational database** for persistence
- A **simple HTML/JavaScript frontend**
- A **data preparation pipeline** for generating synthetic transaction data

---

##  Technology Stack

| Layer | Technologies |
|:------|:--------------|
| **Backend** | Python, Flask, SQLAlchemy |
| **Machine Learning** | Scikit-learn, Pandas, NumPy |
| **Database** | SQLite (default), MySQL (supported) |
| **Frontend** | Vanilla JavaScript (ES6+), HTML5, CSS3 |
| **Environment** | python-dotenv for configuration |

---

##  Features

- 📱 **Responsive web interface** with a dropdown to select a user and a **"Simulate Transaction"** button  
- 🔁 Each click generates a random transaction and sends it to the backend for classification  
- 📊 **Real-time transaction table** displaying details, risk score, and classification  
- 📈 **Summary chart** showing counts of Approved, Flagged, and Fraud transactions per user  
- 🧮 **Live model retraining:**  
  - Retrains a logistic regression model for each user with sufficient transaction history  
  - Falls back to a global model when data is insufficient  
- 💾 **Persistent storage:** all transactions and classifications are stored in a relational database  

---

## 📂 Repository Structure

```

fraud_detection_app/
├── backend/
│   ├── app.py         # Flask application with REST API
│   ├── seed_db.py     # Script to populate the transactions table
│   └── **init**.py    # (empty) marks backend as a Python package
├── dataset/
│   ├── training_data.csv
│   ├── test_data.csv
│   └── simulation_data.csv
├── frontend/
│   ├── index.html     # Main web page
│   ├── script.js      # Frontend logic (fetch API, update UI)
│   └── styles.css     # Minimal styling
├── prepare_data.py     # Generates synthetic dataset
├── requirements.txt    # Python dependencies
├── .env                # Configuration for DB and thresholds
└── README.md           # Project documentation

````

---

##  Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/FraudNix.git
cd FraudNix
````

### 2. Create and Activate a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Generate Synthetic Dataset

```bash
python prepare_data.py
```

This step creates 5,000 synthetic transactions with random users, merchants, and timestamps, then splits them into **training**, **testing**, and **simulation** sets.

### 5. Seed the Database

```bash
python backend/seed_db.py
```

This:

* Populates the transactions table with training data
* Computes a risk score and classification for each transaction
* Saves the results to the database

### 6. Configure the Database (Optional)

SQLite is used by default — no setup needed.
To use MySQL instead, edit `.env`:

```env
DB_URI=mysql+pymysql://user:password@localhost:3306/frauddb
```

Make sure the MySQL server is running and the database exists.

### 7. Run the Backend Server

```bash
python backend/app.py
```

The Flask app runs by default at [http://127.0.0.1:5000](http://127.0.0.1:5000)

### 8. Open the Frontend

Visit [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser.
Select a user and click **Simulate Transaction** to see live classifications.

---

##  Customisation

### • Number of Users

Edit `populateUserSelect` in `frontend/script.js` to change how many users appear in the dropdown (default = 50).

### • Classification Thresholds

Adjust in `.env`:

```env
THRESH_APPROVE=0.3
THRESH_FLAG=0.7
```

Restart the server after making changes.

### • Database Engine

To switch between SQLite and MySQL, update `DB_URI` in `.env`.
Ensure `pymysql` is installed (already listed in `requirements.txt`).

### • Retraining Logic

* Retrains the user-specific model if ≥10 past transactions exist with both fraud and non-fraud cases.
* Otherwise, uses the global model trained on 3,500 transactions.

---

##  Notes & Limitations

* The ML model is **intentionally simple** — it only uses **transaction amount** as a feature.
* In real fraud systems, many features (frequency, merchant type, behavior patterns, etc.) are required.
* Synthetic data generation assigns merchants and users uniformly at random.
* The included `test_data.csv` is not used by default but can be used for evaluation.
* The frontend is lightweight and does **not require a build toolchain** — it uses vanilla JS and the Fetch API.


```

---
`
