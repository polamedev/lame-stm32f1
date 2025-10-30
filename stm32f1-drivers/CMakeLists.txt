


# --------------------------------------------------------------------------------------------------
# Создание библиотеки cmsis-hal

set(ST_HAL_MCU_NAME STM32F103xB CACHE STRING "Тип процесса в HAL")

# Создание цели библиотеки
# Ключевое слово STATIC - говорит о создание статической библиотеке
add_library(stm32f1-drivers STATIC)

target_compile_definitions(stm32f1-drivers 
    PUBLIC 
    USE_HAL_DRIVER
    ${ST_HAL_MCU_NAME}
)

target_include_directories(stm32f1-drivers PUBLIC 
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
if (NOT ST_HAL_SOURCES)
    file(GLOB ST_HAL_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/STM32F1xx_HAL_Driver/Src/*.c")  
endif()
target_sources(stm32f1-drivers 
    PRIVATE 
    ${ST_HAL_SOURCES}
)

# # Подключаем библиотеки 
# # Ключевое слово PUBLIC говорит, что библиотека нужна для сборки цели и для интерфейса данной цели
target_link_libraries(stm32f1-drivers
    PRIVATE
    st_hal_config
)

target_compile_options(stm32f1-drivers PRIVATE -Wno-unused-parameter -Wno-pedantic)

