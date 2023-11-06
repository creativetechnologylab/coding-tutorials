# Reading a Folder of Files with Python

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
for filename in os.listdir(directory_path):
    print(filename)
```

Running this should show a list of the files in the directory that you provided earlier and saved to the `directory_path` variable. If you are not getting the right output, then change `directory_path` so that is leading to the right place.

4. Pass the file names to a command for reading the files.

Once you're satisfied that `os.listdir` is doing the right thing, we can now send these filenames to a command that can take a filename and read the file that is located there. Let's try this out by using a folder of images. In that case we would do the following:

```python
# Create an empty list for the image files
images = []

# Loop through the files in our folder of images
for filename in os.listdir(directory_path):
    # Obtain the combined file path
    combined_image_path = os.path.join(directory_path, filename)
    # Read the image file with cv2
    image = cv2.imread(combined_image_path)
    # Add this image to our image list
    images.append(image)

# Print out our list at the end just to see that it's all worked
print(images)
```

Here we are creating an empty list for our image data. We then loop through the filenames in our image folder and pass these filenames to the `cv2.imread()` command. You will notice that we are not sending the `file_path` from our loop to `imread` but rather the output from a call to the `os.path.join()` command. This is because `os.listdir()` simply gives us the filenames `image-1.png`, `image-2.png`, etc but doesn't actually say where those files _are_. This is why `os.path.join()` is needed to take the path of the folder and the filename of the file we wish to load and _combine_ them. So we are take our `image-1.png` and combine it with `/path/to/image/files` to get `path/to/image/files/image-1.png`. If we gave only the image filenames then we would get an error from `cv2.imread()` unless your working directory _is_ the folder of images.

This is like giving the code the full address to a file. If I say that a package needs to be delivered to 123 Somehwere Street, but I don't tell you the city and country Somewhere Street is in, then you don't know where to go. This is why we need to provide the folder path and filename.

With our file that's been loaded using `cv2.imread()` we can now add it our list by using the `append()` command.

The code above will work provided your image folder _only_ contains valid image files, but it might not. In that case, we need a way of handling cases where `cv2.imread()` is given a file it's unable to open as an image.

5. Handing non-image files in the image folder

It's quite likely you have `.DS_Store` or other random files that have found their way to your folder of images. It's also possible that one or more of your image files have gotten corrupted for whatever reason. In this case we need to instruct Python to carry on with the next file if it encouters a file it's unable to read as an image. To do this we use something called a `try-except` block.

```python
# Create an empty list for the image files
images = []

# Loop through the files in our folder of images
for filename in os.listdir(directory_path):
    # Obtain the combined file path
    combined_image_path = os.path.join(directory_path, filename)
    try:
        # Read the image file with cv2
        image = cv2.imread(combined_image_path)
        # Add this image to our image list
        images.append(image)
    except cv2.error as e:
        print(e)
        print(filename)
        continue

    images.append(image)

print(images)
```

`try-except` means that we can run our code and "catch" any errors or exceptions that get thrown. What this is doing is _attempting_ to read a file as an image. Should this fail we instead print some information about the error that was caused, the file that caused `cv2.imread()` to get the error, and then instruct Python to carry on with the next file by using the `continue` statement. `continue` basically says start this loop again with the next value and don't bother with the rest of the commands that are in the loop. There is no need to add our image file to the list of images if a file failed to load.

Finally you can use the `len()` command to see the length of a list. Use it to find the length of your list of loaded files with `len(images)` (or the name that you have given to your list), and compare that to what your file manager tells you is the number of files in that folder. Do you get the numbers that you expect?