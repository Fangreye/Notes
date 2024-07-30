# Setup GLFW for Visual Studio code from source
## Install gcc and g++ compiler
1. Download MSYS2 from https://www.msys2.org/, and install it.
2. After installation, open MSYS2 MinGW 64-bit terminal.
3. Install the MinGW-w64 toolchain by running the following command in terminal:
```bash
pacman -S --needed base-devel mingw-w64-ucrt-x86_64-toolchain
```
4. Add the following path to the system PATH:
```
E:\MSYS2\ucrt64\bin
```
5. Now check if the gcc and g++ compiler is installed by running the following command in a new cmd terminal:
```bash
gcc --version
g++ --version
```
## Install CMake
1. Download the latest version of CMake from https://cmake.org/download/
2. Run the installer and make sure to add CMake to the system PATH
3. Now check if CMake is installed by running the following command in a new cmd terminal:
```bash
cmake --version
```
## Build GLFW from source
1. Download the GLFW source code from https://www.glfw.org/download.html
2. After extraction, select the folder as 'Where is the source code' in CMake GUI
3. Clear any cmake cache before configuring
4. Specify the generator as 'MinGW Makefiles' and click on 'Configure'
    - You might need Doxygen before configure.
5. After configuration, click on 'Generate'
6. Now create a build folder in the GLFW source code folder
7. Open MSYS2 MinGW 64-bit terminal and navigate to the build folder
8. Run the following command to build GLFW:
```bash
cmake -G "MinGW Makefiles" ..
mingw32-make
```
9. Now you should have a lot of .exe files in the build\examples folder
## Prepare GLAD for GLFW
1. Go to https://glad.dav1d.de/, select the following options, and click on 'Generate':
    - Language: C/C++
    - Specification: OpenGL
    - gl: Version 4.6
    - Profile: Core
2. Download the zip file and extract all files.
3. Now you can include glad.h in your project and link glad.c with your project
## Setup Visual Studio Code
1. Install the C/C++ extension by Microsoft
2. Create a new folder for your project
3. Open VScode through Developer Command Prompt with code . or code-insiders .
4. Hit Ctrl+Shift+P and select 'C/C++: Edit Configurations (JSON)', make the following changes:
```json
    "includePath": [
        "${workspaceFolder}/**",
        "E:/OpenGL/glfw-3.4/include", // Change this path to your GLFW include folder
        "E:/OpenGL/glad/include" // Change this path to your GLAD include folder
    ],
```
5. Create a new file named 'tasks.json' in the .vscode folder and add the following code:
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe build active file",
            "command": "E:\\MSYS2\\ucrt64\\bin\\g++.exe", 
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "E:/OpenGL/glad/src/glad.c", // Change this path to your glad.c
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "-I", "E:/OpenGL/glad/include", // Change this path to yours
                "-I", "E:/OpenGL/glfw-3.4/include", // Change this path to yours
                "-L","E:/OpenGL/glfw-3.4/build/src", // Change this path to yours
                "-lglfw3",
                "-lopengl32",
                "-lgdi32"

            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "Generated task by Debugger."
        }
    ]
}
```
6. Congratulation! Now you can play around with OpenGL in Visual Studio Code. Remember to include glad.h before glfw3.h
