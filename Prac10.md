Practical 10: Edge Detection

import cv2
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
sobelx = cv2.Sobel(gray, cv2.CV_64F, 1, 0, ksize=3)
sobely = cv2.Sobel(gray, cv2.CV_64F, 0, 1, ksize=3)
lap = cv2.Laplacian(gray, cv2.CV_64F)
edges = cv2.Canny(gray, 50, 150)
