Practical 6: Image Restoration and Inpainting

import cv2
mask = cv2.imread("mask.png", 0)
telea = cv2.inpaint(img, mask, 3, cv2.INPAINT_TELEA)
ns = cv2.inpaint(img, mask, 3, cv2.INPAINT_NS)
