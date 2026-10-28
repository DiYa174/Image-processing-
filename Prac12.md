Practical 12: Image Segmentation

import cv2, numpy as np
Z = img.reshape((-1,3)).astype(np.float32)
crit = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 10, 1.0)
_, labels, centers = cv2.kmeans(Z, 4, None, crit, 10, cv2.KMEANS_RANDOM_CENTERS)
_, otsu = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
