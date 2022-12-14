# Makefile for main.bin project (создание проекта из многих файлов)

# для подключения динамической библиотеки см. ссылку ниже
# !!!  https://firststeps.ru/linux/  !!!
# если коротко, то необходимо использовать два варианта
# 1) Записать в файл /etc/ld.so.conf (он ведет в директорию, где лежат файлы для описания мест, где искать дин библиотеки)
# путь, в котором динамический линковщик должен искать вашу библиотеку
# 2) Закинуть путь, где искать вашу библиотеку в переменную среды $LD_LIBRARY_PATH

# Для начала стоит сказать, что объектный файл создаваемый нашим 
# проверенным способом вовсе не подходит для динамических библиотек. 
# Связано это с тем, что все объектные файлы создаваемые обычным образом 
# не имеют представления о том в какие адреса памяти будет
# загружена использующая их программа.
# Несколько различных программ могут использовать одну библиотеку,
# и каждая из них располагается в различном адресном пространстве.
# Поэтому требуется, чтобы переходы в функциях библиотеки
# (операции goto на ассемблере) использовали не абсолютную адресацию,
# а относительную. То есть генерируемый компилятором код должен быть
# независимым от адресов, такая технология получила название PIC - 
# Position Independent Code. 
# В компиляторе gcc данная возможность включается ключом -fPIC.


# Также, с динамическими библиотеками можно работать с помощью библиотеки dl
# Подробнее см: https://firststeps.ru/linux/

# ДЛЯ ИЗМЕНЕНИЯ АРГУМЕНТА ЗАПУСКАЕМОЙ ПРОГРАММЫ ИЗМЕНИТЕ ЗНАЧЕНИЕ VAR 
var=123

main.bin: main.o libfsdyn.so 
	@gcc main.o -ldl -o main.bin
	@./main.bin $(var)

# Создание объектных файлов
main.o: main.c
	@gcc -c main.c

# используем ключ -fPIC 
f1.o: f1.c
	@gcc -fPIC -c f1.c

f2.o: f2.c
	@gcc -fPIC -c f2.c

# Динамическая библиотека это уже не архивный файл, 
# а настоящая загружаемая программа, 
# поэтому созданием динамических библиотек занимается
# сам компилятор gcc.
# Для того, чтобы создать динамическую библиотеку
# надо использовать ключ -shared:
libfsdyn.so: f1.o f2.o
	@gcc -shared -o libfsdyn.so f1.o f2.o

clean:
	@rm -f *.o *.a *.so main.bin

start:
	@./main.bin $(var)
