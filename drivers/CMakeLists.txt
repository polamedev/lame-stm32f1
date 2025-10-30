


# --------------------------------------------------------------------------------------------------
# Создание библиотеки cmsis-hal

set(ST_HAL_MCU_NAME STM32F103xB CACHE STRING "Тип процесса в HAL")

# Создание цели библиотеки
# Ключевое слово STATIC - говорит о создание статической библиотеке
add_library(st_hal STATIC)

target_compile_definitions(st_hal 
    PUBLIC 
    USE_HAL_DRIVER
    ${ST_HAL_MCU_NAME}
)

target_include_directories(st_hal PUBLIC 
    # мои инклуды
    include
    # CMASIS
    CMSIS/Device/ST/STM32F1xx/Include
    CMSIS/Include
    # ST HAL
    STM32F1xx_HAL_Driver/Inc/Legacy
    STM32F1xx_HAL_Driver/Inc
)

# Добавление всех файлов .c в библиотеку
file(GLOB HAL_Src "${CMAKE_CURRENT_SOURCE_DIR}/STM32F1xx_HAL_Driver/Src/*.c")  
target_sources(st_hal 
    PRIVATE 
    ${HAL_Src}
)

# # Подключаем библиотеки 
# # Ключевое слово PUBLIC говорит, что библиотека нужна для сборки цели и для интерфейса данной цели
target_link_libraries(st_hal
    PRIVATE
    st_hal_config
)

target_compile_options(st_hal PRIVATE -Wno-unused-parameter -Wno-pedantic)

