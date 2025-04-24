# Lazur_motor
Lazur motor FOC repo

Как собирать такую штуку используя WSL, VSCode, openocd, gdb и т.д.

1. В WSL ставим openocd b gdb, что-нибудть типа 'sudo apt install openocd gdb -y'
2. По гайдлайну подключаем ST-Link к WSL https://learn.microsoft.com/en-us/windows/wsl/connect-usb
3. Проверяем что все подключилось и установилось командой 'openocd -f interface/stlink-v2.cfg -f target/stm32g4x.cfg' прямо в терминале WSL, должно всё запуститься без ошибок и сказать, что ждет 'gdb'
4. Настраиваем tasks.json и launch.json - возьмите их из этого пакета
Основные идеи взяты отсюда: https://www.zenembed.com/ru/vscode-cubemx-pro-guide 
5. Добавляем модули libcxxcanard и libvoltbro командой 'git submodules add https://github.com/VBCores/libcxxcanard.git VB/libcxxcanard" и "git submodules add https://github.com/VBCores/libvoltbro.git VB/libvoltbro". Если не нравится папка VB делайте свою, но не забудьте поправить CMakeLists.txt там написано из какой папки подключать эти модули. 
6. Без CubeMX всё равно хорошо не соберешь конфиги, так что предположим что .ioc файл у вас есть. Открываем его в CubeMX, выбираем тулчейн CMake и генерируем код в ту директорию в которой будем работать (жмем Generate Code). Называйте проект как надо, проверьте что в сгенерированном CMakeLists.txt всё корректно с названием.
7. Меняем/редактируем CMakeLists.txt пример можно взять из этого пакета
8. Жмем на конфигурирование типа сборки, для начала можно RelWithDebInfo
9. Жмем на билд
10. Жмем на flash
11. Если всё ок жмем на Debug проверяем, что всё работает. 