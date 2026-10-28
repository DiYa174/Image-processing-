Practical 9: Object Detection by Correlation

import cv2
template = img[50:90, 50:90]
res = cv2.matchTemplate(scene, template, cv2.TM_CCOEFF_NORMED)
_, score, _, top_left = cv2.minMaxLoc(res)
cv2.rectangle(scene, top_left, (top_left[0]+40, top_left[1]+40), (0,0,255), 2)
