# Ground Sample Distance (GSD) Estimation for UAV Imagery

![Python](https://img.shields.io/badge/python-3.9-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-orange)
![License](https://img.shields.io/badge/license-MIT-green)

Estimates **Ground Sample Distance** — the real-world size each image pixel represents — for drone/UAV aerial imagery, using linear regression on features extracted from annotated video frames. GSD is the critical link between object size in pixels and real-world object size and orientation.

## Key Results

| Metric | Result |
| --- | --- |
| Train RMSE | 0.0222 |
| Test RMSE | 0.0242 |
| GSD prediction accuracy across objects of varying size, orientation, and altitude | ~89% |

## Method

1. **Frame extraction & preprocessing:** video frames extracted at 30fps, denoised with Gaussian blur (OpenCV).
2. **Annotation:** objects annotated with bounding boxes via CVAT.ai, exported in PASCAL VOC (XML) format.
3. **Feature engineering:** height, width, coordinates, angle from center, and Euclidean/Manhattan distances parsed from the XML annotations; features standardized with StandardScaler.
4. **Dimensionality reduction:** PCA, retaining the first 8 principal components.
5. **Model:** linear regression trained on the PCA-reduced features to predict GSD.

## Repository Structure

```
.
├── Creating CSV file of features/   # frame extraction & feature CSV pipeline
│   ├── frame_extraction.py          # extract frames at 30fps from drone video
│   ├── preprocessing.py             # Gaussian blur denoising (OpenCV)
│   ├── bounding_box.py              # bounding box annotation parsing
│   ├── object_detection.py          # object detection helpers
│   └── resize_frames.py             # frame resizing
├── Creating model on features/      # PCA + linear regression model
│   ├── main.py                      # training / evaluation entry point
│   ├── PCA_calculation.py           # PCA dimensionality reduction (8 components)
│   ├── select_data.py               # data selection
│   ├── video.py                     # video utilities
│   └── object_info_all.csv / object_info_all2.csv   # extracted feature data
├── main code/                       # consolidated pipeline (all of the above)
├── Final Report.pdf                 # full project report
└── README.md
```

## Setup

```
git clone https://github.com/Dp3xD/gsd-estimation-uav-imagery.git
cd gsd-estimation-uav-imagery
pip install -r requirements.txt
```

Requires Python 3.9+, pandas, scikit-learn, OpenCV, and NumPy.

## Usage

```
python features/extract_features.py --annotations data/annotations/
python model/train_gsd.py
```

Trains the PCA + linear regression pipeline and prints train/test RMSE.

## Limitations

- Linear regression assumes a roughly linear pixel-to-ground relationship; breaks down at extreme altitudes or oblique angles
- Annotation quality (CVAT manual boxes) directly bounds model accuracy
- Evaluated on a single drone dataset; camera intrinsics for other UAVs would need recalibration

## Team

Mehul Raval, Hrishikesh Rana, Divya Patel, Prashansa Shah, Aditya Chaudhari — Ahmedabad University

## License

MIT
