# Blink STM32F407VGT6

## Toolchain

[VS Code](https://code.visualstudio.com/)

[Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)

[STM32Cube initialization code generator](https://www.st.com/en/development-tools/stm32cubemx.html)

[STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html)

[cmake](https://cmake.org/download/)

[ninja](https://github.com/ninja-build/ninja/releases)

[STM32CubeCLT](https://www.st.com/en/development-tools/stm32cubeclt.html)

## Action

Add to CMakeLists.txt

```bash
# Execute post-build to print size, generate hex and bin
add_custom_command(TARGET ${CMAKE_PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_SIZE} $<TARGET_FILE:${CMAKE_PROJECT_NAME}>
    COMMAND ${CMAKE_OBJCOPY} -O ihex $<TARGET_FILE:${CMAKE_PROJECT_NAME}> ${CMAKE_PROJECT_NAME}.hex
    COMMAND ${CMAKE_OBJCOPY} -O binary $<TARGET_FILE:${CMAKE_PROJECT_NAME}> ${CMAKE_PROJECT_NAME}.bin
)
```

Create flash.bash to download bin

```bash
#!/bin/bash
set -e

DIR="./build/Release"

cmake --build $DIR --

STM32_Programmer_CLI -c port=SWD -w "$DIR/blink.bin" 0x08000000 -rst
```