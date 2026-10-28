Practical 4: Spatial Domain Image Enhancement

import cv2
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
eq = cv2.equalizeHist(gray)
blur = cv2.GaussianBlur(img, (5,5), 0)
sharp = cv2.filter2D(img, -1, kernel)
_, th = cv2.threshold(gray, 128, 255, cv2.THRESH_BINARY)
