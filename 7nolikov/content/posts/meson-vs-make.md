---
title: Meson vs Make - A simple comparison
date: 2024-12-25
categories: [build]
---

Meson is a modern build system designed to be fast and easy to use. It focuses on simplicity, better configuration management, and improved speed compared to older tools. Meson uses Ninja as its backend to speed up the actual building process, and its configuration files are written in a simple, human-readable language.

<!--more-->

## Comparing Meson with Make

We loved Make. Make has been around for a long time and is well-known in the software development world. It uses Makefiles that contain rules describing how to build the project. While Make is powerful, it can be complex to maintain and sometimes slow when handling large projects.

### Meson offers several advantages over Make

1. **Simplicity**: Meson's configuration files are easier to read and write than complex Makefiles.
2. **Speed**: By using Ninja for the backend, Meson can build projects more quickly.
3. **Modern Features**: Meson includes better dependency management, cross-compilation support, and cleaner syntax.
4. **Error Handling**: Meson provides more understandable error messages, making it easier for developers to troubleshoot issues.

## Usage example

Below is an additional section demonstrating a complex, real-world usage example of Meson. Imagine you have a C++ project that builds a shared library, an executable that depends on that library, and a test suite. With Meson, you can manage these components in a single `meson.build` file. Here's an example:

```meson
project('ExampleProject', 'cpp',
  version: '0.1',
  default_options: ['cpp_std=c++17'])

# Define an include directory for header files.
inc = include_directories('include')

# Build a shared library from the source file.
mylib = shared_library('mylib', 
  sources: ['src/mylib.cpp'],
  include_directories: inc)

# Build an executable that links against the shared library.
example_app = executable('example_app', 
  sources: ['src/main.cpp'], 
  dependencies: [dependency('mylib', fallback: ['.', 'mylib'])],
  include_directories: inc,
  install: true)

# Define a test executable that also uses the shared library.
test_exe = executable('test_example', 
  sources: ['tests/test_example.cpp'],
  dependencies: [dependency('mylib', fallback: ['.', 'mylib'])],
  include_directories: inc)

# Register the test with Meson.
test('example test', test_exe)
```

## Git and Meson

In a recent article on the [Git blog](https://github.blog/open-source/git/highlights-from-git-2-48/#introducing-meson-into-git), Git announced that it has started using Meson for its build system. This change highlights the growing popularity of Meson and its advantages over traditional tools like Make. The Git project is renowned for its complexity and scale, so switching to Meson is a strong endorsement of its efficiency and ease of use.

## Conclusion

Meson is a fresh alternative to Make, offering improved performance, easier configuration, and modern features that simplify the build process. With major projects like Git adopting it, Meson is proving to be a valuable tool in modern software development.

See the [Meson documentation](https://mesonbuild.com/) for getting started with.
