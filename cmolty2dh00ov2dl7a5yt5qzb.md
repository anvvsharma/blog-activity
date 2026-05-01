---
title: "Setting Up a Conda Environment with Python - A Step-by-Step Guide"
datePublished: 2026-04-30T18:41:14.118Z
cuid: cmolty2dh00ov2dl7a5yt5qzb
slug: setting-up-a-conda-environment-with-python-a-step-by-step-guide
cover: https://cdn.hashnode.com/uploads/covers/69ce72fa0ff860b6ded94f66/0e026327-2f4d-4a8e-826e-55e4e9473480.png

---

# Introduction

As we dive into the world of AI and machine learning, it’s essential to have a robust and manageable environment for our projects. In this guide, we’ll walk through the process of creating a new Conda environment with Python3.11, a popular choice among data scientists and developers.

## **Prerequisites**

Before you begin, ensure that you have Conda installed on your system. You can download it from the official Anaconda website.

## Step 1: Create a New Conda Environment

To create a new conda environment with Python3.11, use the following command:

```plaintext
conda create -p day01 python=3.11
```

In this command:

*   **conda create** is used to create a new environment.
    
*   **\-p day01** specifies the name of the environment as `day01`. You can replace `day01` with your desired environment name.
    
*   **python=3.11** specifies the version of Python to be installed in the environment.
    

## Step 2: Review the Package Plan

Once you’ve executed the command, *conda will display a package plan*, outlining the packages that will be installed. Review the output to ensure that Python3.11 is included:

```plaintext
Package Plan
  
environment location: /anvvsharma/agenticai/day01  
  
added / updated specs:  
 - python=3.11  
  
The following NEW packages will be INSTALLED:  
bzip2 pkgs/main/osx-arm64::bzip2–1.0.8-h80987f9_6   
 ca-certificates pkgs/main/osx-arm64::ca-certificates-2025.2.25-hca03da5_0   
 libffi pkgs/main/osx-arm64::libffi-3.4.4-hca03da5_1   
 ncurses pkgs/main/osx-arm64::ncurses-6.4-h313beb8_0   
 openssl pkgs/main/osx-arm64::openssl-3.0.16-h02f6b3c_0   
 pip pkgs/main/noarch::pip-25.1-pyhc872135_2   
 python pkgs/main/osx-arm64::python-3.11.11-hb885b13_0   
 readline pkgs/main/osx-arm64::readline-8.2-h1a28f6b_0   
 setuptools pkgs/main/osx-arm64::setuptools-78.1.1-py311hca03da5_0   
 sqlite pkgs/main/osx-arm64::sqlite-3.45.3-h80987f9_0   
 tk pkgs/main/osx-arm64::tk-8.6.14-h6ba3021_0   
 tzdata pkgs/main/noarch::tzdata-2025b-h04d1e81_0   
 wheel pkgs/main/osx-arm64::wheel-0.45.1-py311hca03da5_0   
 xz pkgs/main/osx-arm64::xz-5.6.4-h80987f9_1   
 zlib pkgs/main/osx-arm64::zlib-1.2.13-h18a0788_1
```

The package plan includes the list of packages that will be installed, including Python3.11.

## Step 3: Activate the Environment

To activate the environment, use the following command:

```plaintext
conda activate /anvvsharma/agenticai/day01
```

You can also use the shorter version:

```plaintext
conda activate day01
```

## Step 4: Verify the Python Version

Once the environment is activated, verify that Python3.11 is installed by running:

```plaintext
python -V
```

You should see the output:

```plaintext
Python 3.11.11
```

## Step 5: Start a Python Interpreter (REPL)

To start a Python interpreter (REPL), simply type:

```plaintext
python
```

In the Python prompt, you can execute Python code. For example:

```plaintext
print("Hello, World!")
```

To exit the Python interpreter, use:

```plaintext
quit()
```

Alternatively, you can create a Python file (e.g., `hello.py`) and run it using:

```plaintext
python hello.py
```

## Conclusion

In this guide, we’ve walked through the process of creating a new Conda environment with Python3.11. By following these steps, you can set up a robust and manageable environment for your AI and machine learning projects. Remember to activate the environment, verify the Python version, and start a Python interpreter (REPL) to begin coding.

For more information, you can refer to the original guide on [GitHub](%5Bhttps://github.com/anvvsharma/agenticai/blob/main/day01/README.md%5D\(https://github.com/anvvsharma/agenticai/blob/main/day01/README.md\)).

## Best Practices

*   Always verify the package plan before proceeding with the installation.
*   Use a consistent naming convention for your environments.
*   Keep your environments organized by using a directory structure.
    

## Troubleshooting

*   If you encounter any issues during the installation process, you can use the `— debug` flag to enable debug mode.
*   If you’re having trouble activating the environment, ensure that you’ve installed Conda correctly and that your environment name is correct.
    

By following these steps and best practices, you can efficiently set up and manage your conda environments with Python.

## Stay Tuned
> Written by [anvvsharma](https://anvvsharma.hashnode.dev)