# Campus Lost & Found Management System

An AI/ML-based Campus Lost & Found Management System built using Python, Streamlit, SQLite and Scikit-Learn.

The system allows students to report lost and found items and automatically identifies possible matches between them.

## Project Objective

Students often lose personal belongings on campus. Usually, lost and found information is shared through WhatsApp groups, notice boards or word of mouth.

This creates a few problems:

* Finding a particular item among many reports is difficult.
* Different people may describe the same item differently.
* Sharing phone numbers publicly can create privacy issues.
* There is no automatic way to compare lost and found reports.

This project provides a simple platform where students can:

* Report lost items
* Report found items
* Find possible matches automatically
* View the reasons for a possible match
* Upload item images
* Send contact requests
* Keep contact details private

## Project Structure

```text
Campus-Lost-and-Found/
│
├── app.py                  # Streamlit application
├── database.py             # SQLite database operations
├── features.py             # Feature calculation
├── matcher.py              # ML matching system
├── generate_dataset.py     # Training dataset generation
├── train_model.py          # Model training
├── README.md               # Project documentation
├── requirements.txt        # Required packages
│
├── uploads/                # Uploaded item images
│
└── data/
    ├── lost_found.db       # SQLite database
    ├── training_data.csv   # Training dataset
    └── model.pkl           # Trained ML model
```

## Machine Learning

The system uses Logistic Regression to identify possible matches between lost and found reports.

Before making a prediction, the system calculates seven features:

| Feature               | Purpose                    |
| --------------------- | -------------------------- |
| `name_similarity`     | Compares item names        |
| `desc_similarity`     | Compares item descriptions |
| `keyword_overlap`     | Checks common keywords     |
| `category_similarity` | Compares item categories   |
| `colour_similarity`   | Compares colours           |
| `location_similarity` | Compares locations         |
| `date_proximity`      | Compares the dates         |

These features are given to the Logistic Regression model.

The model uses `predict_proba()` to calculate the probability of a possible match.

The matching system does not confirm that two items are definitely the same. It only shows potential matches based on the available information.

## Feature Engineering

### Item Name Similarity

TF-IDF with character n-grams is used to compare item names.

### Description Similarity

TF-IDF is used to compare the descriptions provided by users.

### Keyword Overlap

Jaccard similarity is used to compare important words from the reports.

### Category Similarity

The system checks whether the categories of two reports are the same or related.

### Colour Similarity

The system identifies common colour terms and compares them between reports.

### Location Similarity

The system compares the locations mentioned in the two reports.

### Date Proximity

The system gives a higher similarity when the dates of two reports are closer.

## Bidirectional Matching

The matching system works in both directions.

### LOST → FOUND

When a lost report is submitted, it is compared with available found reports.

### FOUND → LOST

When a found report is submitted, it is compared with available lost reports.

The potential matches are sorted based on their predicted probability.

## Privacy System

Phone numbers are not displayed in public reports.

Users can send contact requests when they find a possible match.

The contact request can be:

* Pending
* Accepted
* Declined

Contact details are shared only after the request is accepted.

The identifying details provided by the owner are also kept private.

## Item Images

Users can upload an image along with their lost or found report.

The images are stored in the `uploads/` folder and can be displayed along with the report.

The current Logistic Regression model does not use a trained image recognition model. The main matching process is based on the seven text and attribute-based features.

## Installation

### 1. Open the project

```bash
cd Campus-Lost-and-Found
```

### 2. Install the required packages

```bash
pip install -r requirements.txt
```

### 3. Generate the training dataset

```bash
python generate_dataset.py
```

This creates the training dataset inside the `data/` folder.

### 4. Train the model

```bash
python train_model.py
```

This creates the trained model file inside the `data/` folder.

### 5. Run the application

```bash
streamlit run app.py
```

The application will open through the Streamlit local server.

## Training Dataset

The project uses a synthetic dataset for training the Logistic Regression model.

The dataset contains 1,600 report pairs with matching and non-matching cases.

The dataset is mainly used for demonstrating the working of the ML matching system.

For a real campus deployment, the model could later be trained using properly collected and anonymized campus data.

## Limitations

* The training dataset is synthetic.
* The model has not been trained using a large real-world campus dataset.
* Images are currently stored and displayed but are not used by a trained image classification model.
* The predicted probability does not guarantee that two reports belong to the same item.
* The quality of matching depends on the information entered by the users.

## Future Improvements

* Use real campus lost and found data for training.
* Add image-based matching.
* Add notifications for possible matches.
* Add admin verification.
* Improve the matching model with more advanced NLP techniques.
* Develop a mobile-friendly version.

