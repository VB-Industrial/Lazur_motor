# Lazur_motor
Lazur motor FOC repo

Как собирать такую штуку используя WSL, VSCode, openocd, gdb и т.д.

1. В WSL ставим openocd b gdb, что-нибудть типа 'sudo apt install openocd gdb -y'
2. По гайдлайну подключаем ST-Link к WSL https://learn.microsoft.com/en-us/windows/wsl/connect-usb
3. Проверяем что все подключилось и установилось командой 'openocd -f interface/stlink-v2.cfg -f target/stm32g4x.cfg' прямо в терминале WSL, должно всё запуститься без ошибок и сказать, что ждет 'gdb'
4. Настраиваем tasks.json и launch.json - возьмите их из этого пакета

. Без CubeMX всё равно хорошо не соберешь конфиги, так что предположим что .ioc файл у вас есть. Открываем его в CubeMX, выбираем тулчейн CMake и генерируем код в ту директорию в которой будем работать (жмем Generate Code). Называйте проект как надо, проверьте что в сгенерированном CMakeLists.txt всё корректно с названием. 
3. 