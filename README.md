# glad (Multi-Language GL/GLES/EGL/GLX/WGL Loader-Generator)

[Glad](https://github.com/Dav1dde/glad) is pretty similiar to glLoadGen, it generates a loader for your exact needs based on the official specifications from the Khronos SVN. This means they are always up to date! It was written in a way that you can easily extend it to other languages (e.g. at the date of writing this there are 4 different code generating backends).

You can use the [glad website](https://glad.dav1d.de/) to generate a loader for your needs, download it and use it in your project. Another method of using glad is cloning/downloading the repository and generating your own loader. The tool itself is pretty easy to use and works with any Python version above 2.6, you can also include the source directly into your CMake project.

Glad gives you the option to also generate a very basic loader (similiar to gl3w or glxw), but it is recommended to use the loading function provided by your context creation framework, like `glfwGetProcAddress`. Here is how it looks:

```c++
// glad, include glad *before* glfw
#include <glad/glad.h>
// GLFW
#include <GLFW/glfw3.h>

// ... <snip> ...

int main()
{
    // Init GLFW
    glfwInit();
    // ... <snip> ... setup a window and a context
 
    // Load all OpenGL functions using the glfw loader function
    // If you use SDL you can use: https://wiki.libsdl.org/SDL_GL_GetProcAddress
    if (!gladLoadGLLoader((GLADloadproc) glfwGetProcAddress)) {
        std::cout << "Failed to initialize OpenGL context" << std::endl;
        return -1;
    }
    // Alternative use the builtin loader, e.g. if no other loader function is available
    /*
    if (!gladLoadGL()) {
        std::cout << "Failed to initialize OpenGL context" << std::endl;
        return -1;
    }
    */
  
    // glad populates global constants after loading to indicate,
    // if a certain extension/version is available.
    printf("OpenGL %d.%d\n", GLVersion.major, GLVersion.minor);

    if(GLAD_GL_EXT_framebuffer_multisample) {
        /* GL_EXT_framebuffer_multisample is supported */ 
    }
    if(GLAD_GL_VERSION_3_0) {
        /* We support at least OpenGL version 3 */
    }

    // ... <snip> ... more code
}
```

Glad is able to generate a debugging header, which allows you to hook into your OpenGL calls really easily using `glad_set_pre_callback` and `glad_set_post_callback`, you can find a more detailed guide on the [github repository](https://github.com/Dav1dde/glad#cc-debug).
