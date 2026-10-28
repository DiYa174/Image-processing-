Practical 8: Mathematical Morphology

import cv2
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (3,3))
erosion = cv2.erode(binary, kernel)
dilation = cv2.dilate(binary, kernel)
opening = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)
closing = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
