[image1]: assets/1.png 
[image2]: assets/2.png
[image3]: assets/3.gif
[image4]: assets/4.png 
[image5]: assets/5.png 
[image6]: assets/6.png 
[image7]: assets/7.png 
[image8]: assets/8.png 
[image9]: assets/9.png 
[image10]: assets/10.png 
[image11]: assets/11.png 
[image12]: assets/12.png 
[image13]: assets/13.png 
[image14]: assets/14.png 
[image15]: assets/15.png 
[image16]: assets/16.png 
[image17]: assets/17.png 
[image18]: assets/18.png 
[image19]: assets/19.png 
[image20]: assets/20.png 
[image21]: assets/21.png 
[image22]: assets/22.png 
[image23]: assets/23.png 
[image24]: assets/24.png 
[image25]: assets/25.png 
[image26]: assets/26.png 
[image27]: assets/27.png 
[image28]: assets/28.png 
[image29]: assets/29.png 
[image30]: assets/30.png 
[image31]: assets/31.png 
[image32]: assets/32.png 
[image33]: assets/33.png 
[image34]: assets/34.png 
[image35]: assets/35.png 
[image36]: assets/36.png 
[image37]: assets/37.png 
[image38]: assets/38.png 
[image39]: assets/39.png 
[image40]: assets/40.png 
[image41]: assets/41.png 
[image42]: assets/42.png 
[image43]: assets/43.png 
[image44]: assets/44.png 
[image45]: assets/45.p
[image46]: assets/46.png 
[image47]: assets/47.png 
[image48]: assets/48.png 
[image49]: assets/49.png 


## Content
- [Important libraries](#libraries)
- [Acoustic message](#acc_mess)
- [Path and File Handling - os module](#path_and_file_os)
- [Path and File Handling - pathlib module](#path_and_file_pl)

# Important libraries <a id='libraries'></a>
```python
# standard libraries
import os, sys, time, datetime, random
from glob import glob
from shutil import copyfile
from datetime import datetime
import ast
import itertools

# standard data science libraries
import numpy as np
import pandas as pd
import math

import matplotlib.pyplot as plt
import matplotlib.patches as patches
from matplotlib.patches import Rectangle   
from matplotlib.pyplot import imshow
%matplotlib inline 

# for ImageNet classification using VGG16
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader
from torchvision import models, datasets, transforms
from torch.autograd import Variable
from torchvision.utils import save_image
from torchvision.transforms import ToPILImage

# for images 
import PIL
from PIL import Image
from PIL.ExifTags import TAGS, GPSTAGS
import cv2

# for GMPAS
import gmaps
import gmaps.datasets
gmaps.configure(api_key='...')

# for notebook widgets
import ipywidgets as widgets
from ipywidgets import ToggleButton, RadioButtons, Image, VBox, HTML, Text, HBox, VBox
from ipyfilechooser import FileChooser
from functools import wraps

# for HTML displaying and output
from io import BytesIO
from IPython.display import Markdown, display, HTML
import base64

def printmd(string):
    display(Markdown(string))
    
def pretty_print(df):
    return display( HTML( df.to_html().replace("\\n","<br>") ) )

# check if CUDA is available
train_on_gpu = torch.cuda.is_available()

if not train_on_gpu:
    print('CUDA is not available.  Training on CPU ...')
else:
    print('CUDA is available!  Training on GPU ...')
```

# Acoustic message <a id='acc_mess'></a>
```python
import os
try:
    import win32com.client
    speaker = win32com.client.Dispatch("SAPI.SpVoice")
    speaker.Speak("Let's begin")
except:
    os.system("say 'Let\'s begin")  
else: 
    print('No speak output')
```

# Path and File Handling - os module <a id='path_and_file_os'></a>
## Folder
#### Create new folder
```python
import os 
path = "/tmp/home/monthly/daily/hourly"
try:
    os.mkdir(path)
except:
    pass
```

## Simple Path Handlings
#### Get CWD
```python
import os 
path_cwd = os.getcwd() 
```

#### Create file pathes
```python
import os
os.path.join(path_cwd, 'folder1', 'folder2, 'filename')
```

## Simple File handlings
#### Rename a file
```python
import os 
fd = "Old.txt"
os.rename(fd,'New.txt') 
```
#### Rename files in subdirectories
```python
import os
path = "/path/toyour/folder"
count = 1

for root, dirs, files in os.walk(path):
    for i in files:
        os.rename(os.path.join(root, i), os.path.join(root, "changed" + str(count) + ".txt"))
        count += 1
```

#### Split path, filename, extension
```python
import os

full_file_path = 'C:/Users/test.txt'

file_path =  os.path.splitext(full_file_path)[0]
path, file =  os.path.split(full_file_path)
base = os.path.basename(full_file_path)
base_noExt = os.path.splitext(file)[0]
ext = os.path.splitext(file)[1]

print('file_path ', file_path)
print('path ', path)
print('file ', file)
print('base ', base)
print('base_noExt ', base_noExt)
print('ext ', ext)

('file_path ', 'C:/Users/test')
('path ', 'C:/Users')
('file ', 'test.txt')
('base ', 'test.txt')
('base_noExt ', 'test')
('ext ', '.txt')
```

## Check if file exists
##### File?
```python
import os.path
if os.path.isfile(fname):
    # file exists
```    

#### Path or File?
```python
if os.path.exists(path/file):
    # path/file exists
```
or
```python
try:
    my_abs_path = my_file.resolve(strict=True)
except FileNotFoundError:
    # doesn't exist
else:
    # exists
```


## Copy files
#### Copy with replace
```python
from shutil import copyfile
copyfile(src, dst)
```
- Copy the contents of the file named src to a file named dst.
- The destination location must be writable; otherwise, an IOError exception will be raised.
- If dst already exists, it will be replaced.
- Special files such as character or block devices and pipes cannot be copied with this function.
- With copy, src and dst are path names given as strings.


## Tkinter messagebox
```python
initialdir =
initialfile =
title =
full_file_path = filedialog.asksaveasfilename(initialdir=initialdir, initialfile=initialfile, title =title, filetypes = (("csv files","*.csv"), ("Excel files","*.xlsx"), ("all files","*.*")))
    if file_path !='':
        file_path =  os.path.splitext(full_file_path)[0]
        path, file =  os.path.split(full_file_path)
        ext = os.path.splitext(file)[1]   
        
        ... save file as 
        file_path + ext
```

# Path and File Handling - pathlib module <a id='path_and_file_pl'></a>
## Folder
#### Create new folder
```python
import pathlib as pl
path = pl.Path('temp')
path.mkdir(parents=True, exist_ok=True)
```
- will create a new folder **temp** relative to cwd

## Simple Path Handlings
#### Get CWD
```python
import pathlib as pl
path_cwd = pl.Path.cwd()
```

#### Create file pathes
```python
import pathlib as pl
path = pl.Path.cwd() / 'folder1' / 'output.xlsx'

or

path = pl.Path.cwd().joinpath('folder1').joinpath('output.xlsx')
```

## Simple File handlings
#### Rename a file
```python
import pathlib as pl
myFile = pl.Path('temp/README1.md')
myFile.rename('temp/README2.md')
```

#### Iterate through subdirectories
```python
import pathlib as pl
path = pl.Path('temp')
for i in path.glob('**/*'):
     print(i.name)
```
- will return all files and subfolders in temp

#### Split path, filename, extension
```python
import pathlib as pl
path = pl.Path('c:/foo/bar/setup.py')
print(path.parent)
print(path.parents[0])
print(path.parents[1])
print(path.parents[2])
print(path.name)
print(path.stem)
print(path.suffix)
print(path.is_absolute())
```
- c:/foo/bar
- c:/foo/bar
- c:/foo
- c:
- setup.py
- setup
- .py
- False

## Check if file exists
#### File?
```python
import pathlib as pl
my_file = pl.Path('temp/README3.md')
my_file.is_file()
``` 
- True or False

#### Path?
```python
import pathlib as pl
my_file = pl.Path('temp/README3.md')
my_file.is_dir()
```
- True or False

#### Path or File?
```python

import pathlib as pl
my_file = pl.Path('temp')
my_file.exists()
```
or
```python
try:
    my_abs_path = my_file.resolve(strict=True)
except FileNotFoundError:
    # doesn't exist
else:
    # exists
```