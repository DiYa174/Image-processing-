Practical 11: Colour Space Decomposition

import cv2
rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
ycrcb = cv2.cvtColor(img, cv2.COLOR_BGR2YCrCb)
lab = cv2.cvtColor(img, cv2.COLOR_BGR2LAB)
h, s, v = cv2.split(hsv)
