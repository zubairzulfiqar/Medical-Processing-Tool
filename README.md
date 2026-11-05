# Healthcare Data and Medical Image Processing Tool

A desktop application for managing patient health data, analyzing biomedical signals (ECG/EEG), processing medical images, and creating interactive visualizations.

## Overview
This PyQt5 app loads patient data from CSV, stores it in SQLite, and provides a multi-tab GUI for:
- Patient record management (CRUD)
- Signal analysis (ECG/EEG, FFT)
- Medical image processing with metadata tracking
- Interactive plots, correlation analysis, and heatmaps

## Features

### Foundation & Database
- SQLite database with relational tables: `patients` and `patient_health_metrics`
- Automatic schema creation and migrations
- CRUD operations for patient data
- Clear database action from the UI

### Data Management
- CSV loading and bulk import
- Manual patient entry form (with optional image upload)
- Search by Name or Patient ID
- Update and delete records
- Update image via file dialog when selecting `Original_Image_Data` or `Image_Data`
- Pagination for large datasets

### Health Data Analysis
- Time-series plots with optional raw data overlay
- Outlier removal using adjustable IQR factor
- Correlation analysis (Pearson/Spearman)
- Save correlation results to database

### Signal Processing
- ECG/EEG signal generation on insert if missing
- Load signal from DB by patient
- Raw signal plotting and FFT spectrum analysis
- Save FFT results to patient history and re-load from dropdown

### Image Processing
- Load and display medical images (local or from DB)
- Grayscale, blur, edge detection, thresholding
- Store original + processed images in the database
- Save image metadata (filename, width, height, processing steps)

### Data Visualization
- Scatter plot correlation for selected metrics
- Time-series visualization
- FFT spectrum plotting
- Patient-specific heatmap
- Global heatmap (excludes image dimension columns)

### GUI Tabs
- Data Management
- Health Data Analysis
- Signal Analysis
- Image Processing
- Data Visualization

## Technology Stack
- **Language**: Python 3.8+
- **GUI**: PyQt5
- **Database**: SQLite (`sqlite3`)
- **Data Processing**: NumPy, Pandas, SciPy
- **Signal Processing**: NeuroKit2
- **Image Processing**: OpenCV, Pillow
- **Visualization**: Matplotlib, Seaborn

## Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Setup
```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

pip install -r requirements.txt
```

### Install Dependencies (Standalone Commands)
```bash
pip install pandas numpy scipy matplotlib seaborn PyQt5 opencv-python Pillow neurokit2
```

## Running the Application
```bash
python main.py
```

## Project Structure
```
Medical-Processing-Tool/
+-- main.py                # App entry point
+-- gui_app.py             # PyQt5 GUI and workflows
+-- database_manager.py    # SQLite schema + CRUD + metadata
+-- data_analyzer.py       # Analysis helpers (FFT, ECG/EEG, stats)
+-- insert_thread.py       # Background DB insertion
+-- data_generator.py      # Synthetic CSV generation
+-- styles.qss             # UI styling
+-- requirements.txt       # Python dependencies
+-- heart_disease.csv      # Primary dataset (updated)
+-- ecg_1d_timeseries_prediction.csv
+-- merged_patient_ecg_data.csv
+-- assets/sample_images   # Sample image set for CSV import
+-- health_metrics.db      # SQLite database (runtime-generated/updated)
```

## Usage

### Data Management
1. Open the **Data Management** tab.
2. Load a CSV or enter a patient manually.
3. Use **Update / Delete** to manage records.
4. Use **Search** to filter by Name or Patient ID.
5. Use **Clear Database** to remove all records (with confirmation).

### Health Analysis
1. Select a patient ID or leave blank for global.
2. Pick a time-series metric.
3. Toggle **Show Raw: On/Off** and click **Apply Plot**.
4. Use **Save Correlation** in the Correlation section after computing.

### Signal Analysis
1. Select a patient ID.
2. Choose ECG or EEG in **Biomedical Signal**.
3. Click **Load Signal** and **Compute FFT Spectrum**.
4. Click **Save FFT Record** to store FFT history.
5. Use the FFT history dropdown to re-plot saved spectra.

### Image Processing
1. Load an image (from DB or local file).
2. Apply grayscale/blur/edge/threshold.
3. Save processed results back to the database.

### Data Visualization
1. Enter Patient ID where required.
2. Plot correlation, time-series, FFT, or heatmaps.
3. Use **Patient Heatmap** for a single patient and **Global Heatmap** for the full dataset.

## Dataset Notes
- `heart_disease.csv` includes:
  - `Name`
  - `Date Recorded`
  - `ECG_Signal` and `EEG_Signal`
  - `Image_Path` for sample images
- Sample images are stored in `assets/sample_images/` and are imported into the DB as BLOBs.

## Troubleshooting
- **App fails to start**: Activate your virtual environment and reinstall dependencies.
- **Missing CSV error**: Ensure `heart_disease.csv` exists in the project root.
- **No plots**: Confirm data is loaded and a patient ID is selected.
- **Image tools not working**: Ensure OpenCV is installed (`pip install opencv-python`).

## Notes
- The SQLite database `health_metrics.db` is created/updated automatically.
- Restart the app after schema changes so migrations apply.

## Author
Zubair Zulfiqar
