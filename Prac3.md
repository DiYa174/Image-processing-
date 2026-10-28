Practical 3: 2-D Geometric Transformations

import cv2
h, w = img.shape[:2]
M = cv2.getRotationMatrix2D((w/2, h/2), 30, 1)
rot = cv2.warpAffine(img, M, (w, h))
T = np.float32([[1,0,30],[0,1,30]])
flip = cv2.flip(img, 1)
crop = img[50:200, 50:200

