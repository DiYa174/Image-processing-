Practical 7: Lossless Image Compression

from collections import Counter
pixels = list(gray.flatten())
runs, i = [], 0
while i < len(pixels):
    j = i
    while j < len(pixels) and pixels[j] == pixels[i]: j += 1
    runs.append((pixels[i], j - i)); i = j
