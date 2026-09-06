# Версия CMake
cmake_minimum_required(VERSION 3.15)

#  Плата и микроконтроллер
set(PICO_BOARD pico)
set(PICO_PLATFORM rp2040)

# Подключение SDK
include(pico_sdk_import.cmake)
project(112_blink)
pico_sdk_init()

# Из чего собирать. Типовая команда: создать цель сборки с именем 112_blink из перечисленных исходных файлов. У нас он один.
add_executable(112_blink
    ${CMAKE_CURRENT_SOURCE_DIR}/main.c
)

# С какими библиотеками
# на что опирается ваш код. pico_stdlib — основная библиотека SDK, из неё берутся gpio_init, gpio_put, sleep_ms
target_link_libraries(112_blink
    pico_stdlib
)

# По каким правилам размещать. 
# Специфика микроконтроллера: указать скрипт компоновщика, скопированный на втором шаге.
pico_set_linker_script(112_blink
    ${CMAKE_CURRENT_SOURCE_DIR}/memmap_rp2040.ld
)

# В каких форматах выдать результат. 
# Специфика Pico: кроме основного образа получить файлы .bin и .uf2 — последний понадобится для записи прошивки в плату
pico_add_extra_outputs(112_blink)

# CMake прочитает описание сборки и составит план работ, 
# компилятор переведёт main.c в машинные команды, компоновщик разложит их по адресам из скрипта компоновщика, и на выходе появится файл 112_blink.uf2