# Introduction

This repository **does not** contain any sort of coding implementations for Volume Ray Casting. This only explains how you run an example from some other repository as part of criteria for personal submissions.

Volume Ray Casting is an image-based rendering technic which computes planar (2D) images from volumetric (3D) data.

# The Project Info

I have chosen to explain how you run this repo: https://github.com/khizarsiddiqui/Volume-Rendering.git

The project is in python programming language and has a very poor documentation in regards to setting up the project.

# Project Setup Steps

1. Clone the project

```bash
git clone https://github.com/khizarsiddiqui/Volume-Rendering.git
```

2. Install Python:

The project doesn't say anything about what python version it runs on. I tried running it on the default python version (3.11.1) I had on my laptop and succedded on the first try.

> I prefer using virtual environments with python and used venv to create one for this project.
> ```bash
> # Within the project root
> python -m venv .venv
> .\.venv\Scripts\Activate.ps1
> ```

3. Install Dependencies:

The project depends on a couple of python libraries but there is no description on what's needed. I looked around import statements and error messages when running scripts to find missing dependencies. I installed them using pip and generated this `requirements.txt` file.

```requirements.txt
# requirements.txt

glfw==2.5.9
numpy==1.24.3
Pillow==9.5.0
PyOpenGL==3.1.7
scipy==1.10.1
```

```bash
pip install -r <path_to>requirements.txt
```

> After installing dependencies, there were some problems which is probably because of different version of glfw from the code author.
> I updated some args on some glfw functions and changed a few things, after which my git diff looks like this.
```patch
diff --git a/raycube.py b/raycube.py
index fd70b38..e166513 100644
--- a/raycube.py
+++ b/raycube.py
@@ -268,7 +268,7 @@ class RayCube:
         """clears old FBO"""
         # delete FBO
         if glIsFramebuffer(self.fboHandle):
-            glDeleteFramebuffers(int(self.fboHandle))
+            glDeleteFramebuffers(1, int(self.fboHandle))
     
         # delete texture
         if glIsTexture(self.texHandle):
diff --git a/volrender.py b/volrender.py
index 85c8b54..68ecaca 100644
--- a/volrender.py
+++ b/volrender.py
@@ -27,9 +27,9 @@ class RenderWin:
         glfw.glfwWindowHint(glfw.GLFW_OPENGL_PROFILE, glfw.GLFW_OPENGL_CORE_PROFILE)
 
         # make a window
-        self.width, self.height = 512, 512
+        self.width, self.height = 1024, 1024
         self.aspect = self.width/float(self.height)
-        self.win = glfw.glfwCreateWindow(self.width, self.height, b"volrender")
+        self.win = glfw.glfwCreateWindow(self.width, self.height, "volrender", None, None)
         # make context current
         glfw.glfwMakeContextCurrent(self.win)
         

```

4. Running the Code:

The code doesn't have a main.py file like people normally create. Instead, I searched for `__main__` in all the project files and found two scripts that could be run as a python script. `makedata.py` and `volrender.py`.

4.1. `makedata.py` seems to generate 256 png files. This is optional because repository already has the png files.

```bash
python .\makedata.py
```

4.2. `volrender.py` seems to depend on other py files on the project and display a OpenGL window with two 3D shapes: A sphere and a cuboid.

```bash
python .\volrender.py --dir .\sphere-cuboid\
```

# Controls

The window opened from `volrender.py` has some controls, which I  discovered upon inspecting the code.

1. Pressing `v` toggles the view mode (Slice Render / Ray Cast Render)
2. Pressing right or left arrow will sort of rotate the image in one mode and zoom the sphere in another.
3. Pressing `esc` key will close the window and stop the application
4. When on Slice Render Mode, Pressing x, y, z will change the perspective.

# Demonstration

[VolRender.webm](https://github.com/sanjeevmaharjan/volume-ray-casting/assets/10544486/471eafb8-5808-4528-937e-1778f5481524)

