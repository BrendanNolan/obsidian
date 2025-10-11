The usual structure of a cmake project is to have a `build` dir and to build as follows:

- `cd build` and then `cmake ..` to configure the build
- `cmake --build .` to run the build
- The executable will then be at `build/<program_name>`
