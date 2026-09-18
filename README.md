# smart-document-scanner
An intelligent computer vision application that transforms skewed, handheld photographs of documents into clean, flat, and professional digital scans.
1. Files Created & Repository Structure
text


cv project/
├── main.py                     # Primary modular CLI scanner application
├── requirements.txt            # Minimal required dependencies (opencv-python, numpy)
├── README.md                   # 22-section academic documentation & user guide
├── REPORT.md                   # 18-section formal course project report with ASCII pipeline
├── .gitignore                  # Ignores venv/, caches, output/, and editor configs
└── sample/
    ├── README.md               # Guide to sample test images
    ├── generate_samples.py     # Non-copyrighted synthetic test image generator
    ├── sample_document.jpg     # Realistic perspective-tilted document test image
    └── sample_no_document.jpg  # Negative test image (no 4-corner document)
Clean Working Tree: The repository has been initialized with git and committed (Initial commit: Smart Document Scanner using OpenCV academic project). venv/ and generated output/ images are ignored by 
.gitignore
.
2. How to Run the Project
Virtual Environment Setup & Dependencies
bash


# 1. Create and activate a virtual environment
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
# 2. Install dependencies
pip install -r requirements.txt
Running the Document Scanner
bash


# Default execution (saves outputs to output/ directory)
python main.py --input sample/sample_document.jpg
# Custom output file path
python main.py --input sample/sample_document.jpg --output scanned_doc.jpg
# Custom output directory
python main.py --input sample/sample_document.jpg --output-dir results/
Generated Outputs
When run on a document image, the program automatically saves:

output/edge_detection.jpg — Canny edge map with morphological closing.
output/perspective_corrected.jpg — True-color top-down rectified document.
output/scanned_document.jpg — High-contrast black-and-white scanned document via adaptive thresholding.
Regenerating Test Images
bash


python sample/generate_samples.py
3. Dependencies
Specified in 
requirements.txt
:

opencv-python>=4.8.0 (tested with OpenCV 5.0.0.93)
numpy>=1.24.0 (tested with NumPy 2.5.3)
4. Computer Vision Concepts Demonstrated
Aspect-Ratio Preserved Resizing: Normalizes scale for uniform gradient calculations while scaling coordinates back to original resolution.
Grayscale Reduction: Converts 3-channel BGR to 1-channel luminance (
Y
=
0.299
R
+
0.587
G
+
0.114
B
Y=0.299R+0.587G+0.114B).
Gaussian Smoothing: 
5
×
5
5×5 spatial convolution kernel for noise suppression.
Canny Edge Detection & Morphological Structuring: Gradient magnitude/direction estimation, non-maximum suppression, and hysteresis thresholding ($75, 200
)
w
i
t
h
a
$
3
×
3
)witha$3×3 morphological close.
Ramer-Douglas-Peucker Polygon Approximation: cv2.approxPolyDP with epsilon tolerance (
0.02
×
Perimeter
0.02×Perimeter) and convexity filtering (cv2.isContourConvex) to isolate the 4-corner document boundary.
Topological Point Ordering: Vectorized coordinate sorting into [Top-Left, Top-Right, Bottom-Right, Bottom-Left] using coordinate sums and differences.
Planar Homography / Perspective Warp: Computes maximum Euclidean edge dimensions and applies cv2.getPerspectiveTransform and cv2.warpPerspective.
Adaptive Gaussian Thresholding: Computes localized 
21
×
21
21×21 neighborhood thresholds to neutralize shadows and illumination gradients without requiring neural networks.
5. Testing Performed & Validation Results
Test Case	Command	Expected Result	Actual Result	Status
Valid Document	python main.py --input sample/sample_document.jpg	Detects 4 corners, warps perspective, saves 3 outputs	4 corners detected within 0.5px accuracy, created scanned_document.jpg, perspective_corrected.jpg, edge_detection.jpg	PASSED (Exit 0)
Custom Output Path	python main.py --input sample/sample_document.jpg --output custom/scan.jpg	Writes to custom folder with derived companion filenames	Created custom/scan.jpg, scan_color.jpg, scan_edges.jpg	PASSED (Exit 0)
Missing Image File	python main.py --input does_not_exist.jpg	Informative error message without crash	[ERROR] Input image file not found: does_not_exist.jpg	PASSED (Exit 1)
Negative Test (No Document)	python main.py --input sample/sample_no_document.jpg	Handles absence of 4-corner polygon gracefully	[ERROR] Document boundary not detected: No suitable four-corner contour found. with diagnostic tips	PASSED (Exit 1)
6. Items Needing Your Attention
Remote GitHub Repository: The local git repository has been initialized and committed with a clean working tree. If you want to push this to GitHub, simply configure your remote URL:
bash


git remote add origin <your-github-repo-url>
git branch -M main
git push -u origin main
