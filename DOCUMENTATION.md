# Software Documentation - Healthcare Data and Medical Image Processing Tool

## 1. Purpose
This application manages patient health data, analyzes biomedical signals (ECG/EEG), processes medical images, and visualizes results through an interactive PyQt5 GUI. It supports CSV import, database storage, filtering, correlation analysis, FFT spectrum analysis, and image processing workflows.

## 2. Target Users
- Students and researchers working with healthcare datasets
- Instructors evaluating data processing, visualization, and database integration
- Analysts exploring patient metrics and biomedical signals

## 3. System Requirements
- Python 3.8+
- Windows, macOS, or Linux
- Packages listed in `requirements.txt`

## 4. Installation
```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

pip install -r requirements.txt
```

## 5. Application Architecture

### 5.1 Core Modules
- `main.py`: Application entry point and bootstrap.
- `gui_app.py`: GUI layout, user interactions, and tab logic.
- `database_manager.py`: SQLite schema, CRUD operations, and metadata handling.
- `data_analyzer.py`: Signal processing, FFT analysis, and filters.
- `insert_thread.py`: Background thread for bulk inserts.
- `data_generator.py`: Synthetic dataset generator.

### 5.2 Data Storage
- SQLite database file: `health_metrics.db`
- Primary dataset: `heart_disease.csv`
- Images: stored as BLOBs in DB, sourced from `assets/sample_images/`

### 5.3 Database Tables (Logical Overview)
- `patients`: master record of patient identifiers and static details.
- `patient_health_metrics`: time-series health metrics, signals, images, and analysis results.

## 6. UI Overview (Tabs)

### 6.1 Data Management
- Import CSV and insert into DB
- Manual patient entry (including optional image upload)
- Search by Name or Patient ID
- Update/Delete records
- Clear database with confirmation
- Paginated table view

### 6.2 Health Data Analysis
- Time-series plotting for a selected metric
- Optional raw data overlay toggle
- IQR-based outlier removal (slider)
- Correlation analysis (Pearson/Spearman)
- Save correlation results to DB

### 6.3 Signal Analysis
- Load ECG/EEG data for a patient
- Plot raw signal
- Compute FFT spectrum
- Save FFT results per patient
- Load saved FFT history from dropdown

### 6.4 Image Processing
- Load image from DB or local system
- Apply grayscale, blur, edge detection, thresholding
- Save processed image back to DB
- Store metadata: filename, width, height, processing steps

### 6.5 Data Visualization
- Plot correlation (scatter)
- Plot time-series
- Plot FFT spectrum
- Patient-specific heatmap
- Global heatmap (excludes image dimension columns)

## 7. Data Flow
1. CSV imported into DB.
2. Signals/images are validated or generated when missing.
3. Analysis runs on DB-backed records.
4. Outputs (correlations, FFT data, processed images) are stored for later retrieval.

## 8. Signal Processing Details
- ECG/EEG signals are stored as serialized numeric lists.
- FFT uses SciPy/Numpy frequency domain transforms.
- FFT results can be saved and retrieved per patient.

## 9. Image Processing Details
- Images are processed using OpenCV.
- Original and processed images are stored as BLOBs.
- Metadata is stored for auditing and UI display.

## 10. Common Workflows

### 10.1 Import and Analyze Data
1. Load CSV in Data Management tab.
2. Pick a patient in Health Analysis.
3. Plot time-series, apply filters.
4. Compute and save correlations.

### 10.2 Signal Analysis
1. Select patient and signal source (ECG/EEG).
2. Load signal and compute FFT.
3. Save FFT record.
4. Re-open FFT from history list.

### 10.3 Image Processing
1. Load image from DB or file.
2. Apply processing filters.
3. Save processed image.

## 11. Known Behaviors
- Heatmaps exclude `Image_Width` and `Image_Height` to keep correlations focused on health metrics.
- Clearing DB removes all records and resets views.

## 12. Troubleshooting
- **No data in plots**: Ensure CSV is loaded and patient ID is selected.
- **Images not loading**: Verify image path in CSV or upload image manually.
- **FFT history empty**: Save FFT after computation before reloading.

## 13. Author
Zubair Zulfiqar
