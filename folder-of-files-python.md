# Reading a Folder of Files with Python

In this example, we will demonstrate how to use Python to load a folder of images, by using the `cv2` library. The goal is to read all the image files contained in a specific folder, but you can adapt this code to reading some other file type, or passing a collection of images to another command.

## 1. Import Required Libraries

We need to use the `os.listdir()` (list directory) function to list all the files in a given folder. In order to do this we must first import the `os` module. The `os` module offers a wide range of functionalities for interacting with the operating system, including managing directories and files.

```python
import os
import cv2
```

## 2. Specify the Directory Path

You need to provide the path to the directory you want to read. This path can be either an absolute path or a relative path. In the case of a relative path, it is relative to the current working directory. For an absolute path, it should represent the full path to the folder from the root directory of your file system. For Windows users, an absolute path typically looks like `C:\Users\User\...`, while for Mac/Linux users, it appears as `/home/user/...`.

```python
directory_path = '/path/to/your/directory/of/files'
```

## 3. Use `os.listdir()` to Obtain a List of Files and Directories

Once you have defined the `directory_path`, you can use the `os.listdir()` function to retrieve a list of file and directory names within that specified directory. For now, we will print these names to verify that everything is functioning correctly.

```python
for filename in os.listdir(directory_path):
    print(filename)
```

Executing this code will display a list of files in the directory you provided earlier, which is stored in the `directory_path` variable. If the output is not as expected, ensure that `directory_path` points to the correct location.

## 4. Read the Files

Once you are confident that `os.listdir` is functioning correctly, you can proceed to read these filenames using a command capable of reading the files. In this example, we will work with a folder of images. Here's how to do it:

```python
# Create an empty list to store the image files
images = []

# Iterate through the files in our image folder
for filename in os.listdir(directory_path):
    # Obtain the complete file path
    combined_image_path = os.path.join(directory_path, filename)
    # Read the image file with cv2
    image = cv2.imread(combined_image_path)
    # Add the image to our list of images
    images.append(image)

# Display the list at the end to ensure everything has worked
print(images)
```

In this code, we create an empty list to store image data. We then loop through the filenames in our image folder and pass these filenames to the `cv2.imread()` function. Note that we use `os.path.join()` to combine the folder path with the filename to obtain the full file path. This is necessary because `os.listdir()` only provides filenames without specifying their location. Providing the full path allows `cv2.imread()` to locate the files. If you only provide the image filenames, you may encounter errors with `cv2.imread()` unless your working directory is the same as the image folder.

By loading the image using `cv2.imread()`, we can add it to our list using the `append()` function.

The provided code works as long as your image folder contains valid image files. However, if there are non-image files or corrupted files, we will need to add some extra code to handle this.

## 5. Handling Invalid Files in the Folder

It's common to encounter files like `.DS_Store` or other non-image files in your folder. Additionally, some files may be corrupted. In such cases, Python should continue with the next file if it encounters a file it cannot read. To achieve this, you can use a `try-except` block:

```python
# Create an empty list to store the image files
images = []

# Iterate through the files in our image folder
for filename in os.listdir(directory_path):
    # Obtain the complete file path
    combined_image_path = os.path.join(directory_path, filename)
    try:
        # Read the image file with cv2
        image = cv2.imread(combined_image_path)
        # Add the image to our list of images
        images.append(image)
    except cv2.error as e:
        print(e)
        print(filename)
        continue

print(images)
```

The `try-except` block allows you to run the code and capture any errors that occur within the `try` block. If reading a file as an image fails, the code prints information about the error, the file that caused the error, and instructs Python to continue with the next file using the `continue` statement. There is no need to add the image file to the list if it fails to load.

Finally, you can use the `len()` function to check the length of the list. Use `len(images)` (or the name of your list) to determine how many files were successfully loaded and compare it to the number of files your file manager reports in the folder. Do you get the numbers that you expect?