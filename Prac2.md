Practical 2: Image Format Conversion, Arithmetic and Bitwise Operations

import cv2
img = cv2.imread("input.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
added = cv2.add(img, 60)
inv = cv2.bitwise_not(img)
and_ = cv2.bitwise_and(img, img, mask=mask)
