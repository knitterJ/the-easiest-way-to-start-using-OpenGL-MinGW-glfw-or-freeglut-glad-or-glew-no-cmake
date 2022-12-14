
## What are OpenGL loading libraries?

What's interesting, OpenGL is preinstalled in your Windows, but it's still necesary to configure a loading library. Loading library is responsible for loading pointers to OpenGL functions at a runtime. This way, you don't need to load OpenGL functions manually. If you want to/need to configure GLEW instead of Glad go [here]().

## GLAD configuration

1/5) Go to [official Glad website](https://glad.dav1d.de/) and generate GLAD for the newest version of OpenGL (OpenGL 4.6 was released in 2017, so I assume that you don't need to download earlier version). Click on glad.zip in order to start download and then unzip the package.
![1/5](https://github.com/knitterJ/the-easiest-way-to-start-using-OpenGL-MinGW-glfw-or-freeglut-glad-or-glew-no-cmake/blob/main/Windows/3.GLAD-or-GLEW-alternatively/1.png)

2/5) Enter the newly created folder and copy 2 folders located in the `include` folder (meaning KHR and glad) to `C:\MinGW\include`

![2/5](https://github.com/knitterJ/the-easiest-way-to-start-using-OpenGL-MinGW-glfw-or-freeglut-glad-or-glew-no-cmake/blob/main/Windows/3.GLAD-or-GLEW-alternatively/2.gif)

3/5) Open new command prompt and navigate to `src` folder where glad.c file is located. Then introduce the following command:

`gcc -c glad.c`

and right after the next command:

`ar rcs libglad.a glad.o`

![3/5](https://github.com/knitterJ/the-easiest-way-to-start-using-OpenGL-MinGW-glfw-or-freeglut-glad-or-glew-no-cmake/blob/main/Windows/3.GLAD-or-GLEW-alternatively/3.gif)

This way static library (libglad.a), which can be used universally in every project, was created.

4/5) Move libglad.a file to `C:\MinGW\lib`

![4/5](https://github.com/knitterJ/the-easiest-way-to-start-using-OpenGL-MinGW-glfw-or-freeglut-glad-or-glew-no-cmake/blob/main/Windows/3.GLAD-or-GLEW-alternatively/4.png)

Now it's possible to compile C++ programs with the reference to `-lglad` library

5/5) Test the configuration with the following file (downloadable easily from [here](https://onlinegdb.com/jQ7vm30xU)):

```
#include <glad/glad.h>
#include <GLFW/glfw3.h>
#include <iostream>

void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow *window);

const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{   
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

#ifdef __APPLE__
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
#endif

    GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }    

    while (!glfwWindowShouldClose(window))
    {

        processInput(window);
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);
        glfwSwapBuffers(window);
        glfwPollEvents();
    }
    glfwTerminate();
    return 0;
}
void processInput(GLFWwindow *window)
{
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}
```

 `g++ main.cpp -lglad -lglfw3dll`

![](https://github.com/knitterJ/the-easiest-way-to-start-using-OpenGL-MinGW-glfw-or-freeglut-glad-or-glew-no-cmake/blob/main/Windows/3.GLAD-or-GLEW-alternatively/5.png)

This way you configured successfully libraries, which allows to use OpenGL.
If you want to compile your OpenGL programs with a hit of a button, go to the bonus section, where I explain [how to configure Atom text editor](https://github.com/knitterJ/the-easiest-way-to-start-using-OpenGL-MinGW-glfw-or-freeglut-glad-or-glew-no-cmake/tree/main/Windows/4.Bonus-atom-text-editor-configuration).
