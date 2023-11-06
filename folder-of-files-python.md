# Processing a Folder of Files in Python

For this example we're going to use Python to load a folder of images. Let's say we want to load all our images into Python with a library like `cv2`. In this guide we will show you how to enable Python to read all the image files contained in a folder.

1. Import the `os` module and any other necessary libraries.

Before you can use `os.listdir`, you need to import the `os` module. This module provides a wide range of functions for interacting with the operating system, including working with directories and files.

```python
import os
import cv2
```

2. Specify the directory path:

You need to provide the path of the directory you want to read. This path can be either an absolute path or a relative path. In the case of a relative path, it's relative to the current working directory. For an absolute path, this is the full path for the folder starting from the root directory of your file system. For Windows users, an absolute path will typically take the form `C:\Users\User\...` while for Mac/Linux users an absolute path will take the form `/home/user/...`.

```python
directory_path = '/path/to/your/directory/of/files'
```

3. Use `os.listdir` to get a list of files and directories:

Once you've defined the `directory_path`, you can use `os.listdir` to give a list the names of all the files in that directory. For now, let's just print their names so we can be assured that everything is working:

```python
for file_path in os.listdir(directory_path):
    print(file_path)
```

Running this should show a list of the files in the directory that you provided earlier and saved to the `directory_path` variable.

4. Pass the file names to a command.

Once your satisfies that `os.listdir` is doing the right thing, we can now send these filenames to a command that can take a filename and read the file that is located there. Let's try this out by using a folder of images. In that case we would do the following:

```python
images = []

for file_path in os.listdir(directory_path):
    combined_image_path = os.path.join(directory_path, file_path)
    images.append(cv2.imread(combined_image_path))
```

Here we are creating an empty list for our image data. We then loop through the filenames in our image folder and pass these filenames to the `cv2.imread()` command. You will notice that we are not sending the `file_path` from our loop to `imread` but rather the output from a call to the `os.path.join()` command. 
