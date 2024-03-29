# Checks if colors are in image / Detects multiple colors in images - Fast Cython algorithm

### pip install cythoncolortools

#### Tested against Windows 10 / Python 3.11 / Anaconda

### Important!
The module will be compiled when you import it for the first time. 
Cython and a C/C++ compiler must be installed!

### How to use it in Python 

```python
import numpy as np
import cv2
from cythoncolortools import search_colors, are_any_colors_in_picture

# 4525 x 6623 x 3 picture https://www.pexels.com/pt-br/foto/foto-da-raposa-sentada-no-chao-2295744/
picpath = r"C:\Users\hansc\Downloads\pexels-alex-andrews-2295744.jpg"
pic = cv2.imread(picpath)
colors0 = np.array([[255, 255, 255]], dtype=np.uint8)
resus0 = search_colors(pic=pic, colors=colors0)
colors1 = np.array(
    [
        (66, 71, 69),
        (62, 67, 65),
        (144, 155, 153),
        (52, 57, 55),
        (127, 138, 136),
        (53, 58, 56),
        (51, 56, 54),
        (32, 27, 18),
        (24, 17, 8),
    ],
    dtype=np.uint8,
)
resus1 = search_colors(pic=pic, colors=colors1)
print(resus1)


# %timeit search_colors(pic=pic, colors=colors1, add_results=True, cpus=5)
# %timeit search_colors(pic=pic, colors=colors1, add_results=False, cpus=5)

# %timeit search_colors(pic=pic, colors=colors0, add_results=True, cpus=5)
# %timeit search_colors(pic=pic, colors=colors0, add_results=False, cpus=5)

# %timeit search_colors(pic=pic, colors=colors1, add_results=True, cpus=1)
# %timeit search_colors(pic=pic, colors=colors1, add_results=False, cpus=1)

# %timeit search_colors(pic=pic, colors=colors0, add_results=True, cpus=1)
# %timeit search_colors(pic=pic, colors=colors0, add_results=False, cpus=1)
print(search_colors(pic=pic, colors=colors1, add_results=True, cpus=5))
print(search_colors(pic=pic, colors=colors1, add_results=False, cpus=5))
print(search_colors(pic=pic, colors=colors0, add_results=True, cpus=5))
print(search_colors(pic=pic, colors=colors0, add_results=False, cpus=5))
print(search_colors(pic=pic, colors=colors1, add_results=True, cpus=1))
print(search_colors(pic=pic, colors=colors1, add_results=False, cpus=1))
print(search_colors(pic=pic, colors=colors0, add_results=True, cpus=1))
print(search_colors(pic=pic, colors=colors0, add_results=False, cpus=1))


print(are_any_colors_in_picture(pic, colors1, cpus=-1))
print(are_any_colors_in_picture(pic, colors0, cpus=-1))
print(are_any_colors_in_picture(pic, colors1, cpus=1))
print(are_any_colors_in_picture(pic, colors0, cpus=1))

print(are_any_colors_in_picture(pic, [[111, 111, 121]], cpus=-1))
print(are_any_colors_in_picture(pic, [[111, 111, 121]], cpus=1))

# 57 ms ± 2.9 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
# 47.9 ms ± 1.02 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
# 22 ms ± 43.6 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)
# 18.8 ms ± 162 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
# 260 ms ± 8.03 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
# 256 ms ± 283 µs per loop (mean ± std. dev. of 7 runs, 1 loop each)
# 25.7 ms ± 47.2 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)
# 25.8 ms ± 110 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

# [[  38    0  136  138  127]
#  [   1    1  153  155  144]
#  [  40    1  153  155  144]
#  ...
#  [1973 5903   65   67   62]
#  [1952 5904   65   67   62]
#  [2868 6041   65   67   62]]
# [[4522    0   69   71   66]
#  [   1    1  153  155  144]
#  [  40    1  153  155  144]
#  ...
#  [4522 6622    8   17   24]
#  [4523 6622    8   17   24]
#  [4524 6622    8   17   24]]
# [[  38    0]
#  [4522    0]
#  [   1    1]
#  ...
#  [2844 6622]
#  [2854 6622]
#  [2865 6622]]
# [[2085  832  255  255  255]
#  [1692  858  255  255  255]
#  [1688  896  255  255  255]
#  ...
#  [3526 5491  255  255  255]
#  [3527 5491  255  255  255]
#  [2491 5525  255  255  255]]
# [[2085  832]
#  [1692  858]
#  [1688  896]
#  ...
#  [3526 5491]
#  [3527 5491]
#  [2491 5525]]
# [[4522    0   69   71   66]
#  [4522    3   69   71   66]
#  [4523    3   69   71   66]
#  ...
#  [4522 6622    8   17   24]
#  [4523 6622    8   17   24]
#  [4524 6622    8   17   24]]
# [[4522    0]
#  [4522    3]
#  [4523    3]
#  ...
#  [4522 6622]
#  [4523 6622]
#  [4524 6622]]
# [[2085  832  255  255  255]
#  [1692  858  255  255  255]
#  [1688  896  255  255  255]
#  ...
#  [3526 5491  255  255  255]
#  [3527 5491  255  255  255]
#  [2491 5525  255  255  255]]
# [[2085  832]
#  [1692  858]
#  [1688  896]
#  ...
#  [3526 5491]
#  [3527 5491]
#  [2491 5525]]
# True
# True
# True
# True
# False
# False
```