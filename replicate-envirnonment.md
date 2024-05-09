# Replicating an Environment

If the machine you're exhibiting with differs from the one you used for development, you'll need to replicate your Python environment on the presentation machine. To do this, we create either a requirements.txt or an environment.yml file that lists all the dependencies your project relies on and the specific versions of those dependencies that is required. This file can then be used to install the exact same dependencies on your presentation machine, ensuring that your code runs as expected in the exhibition environment.

## Conda / Mamba

If you developed your code while using a Conda or Mamba environment, then the process simply requires creating an environment.yml file. This contains a list of the libraries used and their versions making it easy to duplicate this environment on another machine. To create such a file, first made sure that the environment you wish to replicate is active, and then execute the following command in either the terminal (Mac) or the Miniforge Prompt (Windows):

Mamba: `mamba list -e > environment.yaml`  
Conda: `conda list -e > environment.yaml`

This will cause an `environment.yaml` file to be created in the current working directory. If you are using Git, this is a helpful file to add to your repository.

To then recreate the environment from scatch, you then use the following commands:

Mamba: `mamba env create --file environment.yaml`  
Conda: `conda env create --file environment.yaml`

Now, as you would with any other environment, you can `activate` it and begin using code that requires the libraries contained within that environment.

## `requirements.txt`

If you didn't use Conda or Mamba, then we can create a similar file simply by using pip. This can be done using the following command within a terminal:

`pip freeze > requirements.txt`

This will create a `requirements.txt` file that also contains a list of the Python libraries that you have installed and their required versions. This will only record libraries that were installed via pip, so if anything was installed with Conda/Mamba, you should instead use the method above to ensure that no libraries have been left out.

This is also a file worth tracking with Git if you are using it.

Now in the presentation machine, you can duplicate your command using the command below:

`pip install -r requirements. txt`

Pip will then install all the packages that are in the file.

## Operating System Switch

It is important to note that certain libraries behave differently and/or need to be set up differently on various operating systems. If you are switching from one OS to another for your presentation, then you will need to check this ahead of time and perhaps make some changes to what libraries are being used or their versions. You could keep this organised by naming your files `requirements-mac.txt`, `requirements-win.txt`, etc to avoid confusion.