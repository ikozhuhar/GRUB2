### GRUB2. Загрузка системы в Linux

#### <a name='toc'>Содержание</a>

1. [Включить отображение меню Grub](#entrymenugrub)
2. [Попасть в систему без пароля несколькими способами](#&&&&&&&&&&&&&)
3. [Установить систему с LVM, после чего переименовать VG](#&&&&&&&&&&)
4. [Дополнительные источники](#recommended_sources)


#### 1. [[⬆]](#toc) <a name='entrymenugrub'>Включить отображение меню Grub</a>

#####  Включить отображение меню Grub
Чтобы показать меню GRUB можно при загрузке зажать:

1. клавишу Shift (на компьютерах с BIOS)
2. клавишу Esc (для современных компьютеров с UEFI)

Если это не помогло, нужно отредактировать конфигурационный файл GRUB. Загрузитесь в Linux и включите отображение меню GRUB, добавив (раскомментировав) следующие строки в /etc/default/grub:
```
sudo nano /etc/default/grub
GRUB_TIMEOUT=20
```
![image](https://github.com/user-attachments/assets/f1cfc73a-84a9-4e7b-bbc1-fc762660a1a2)

Эта опция включает таймаут 20 секунд, которые должен ждать GRUB при загрузки на этапе выбора операционной системы.

Проверьте, есть ли в конфиг файле строка:
```
GRUB_TIMEOUT_STYLE=hidden
```

Если такая строка есть, закоментируйте ее или измените на
```
GRUB_TIMEOUT_STYLE=menu
```
После изменения настроек в файле grub нужно обновить его конфигурацию командой:
```
sudo update-grub
```
![image](https://github.com/user-attachments/assets/eb5225fd-4b8e-427d-89ce-5b329cd73f59)

Перезагрузите компьютер.

Если меню GRUB все еще не показывается, возможно GRUB не поддерживает видео режим вашего графической адаптера. Вы можете вместо графического GRUB меню отобразить консольное меню. Для этого добавьте в файл `etc/default/grub` строку:
```
GRUB_TERMINAL=console
```

Сохраните файл и обновите конфигурацию
```
sudo update-grub
```
Перезагрузитесь и убедитесь, что GRUB теперь показывает загрузочное меню.  

![image](https://github.com/user-attachments/assets/ae603526-9bbe-44c9-8394-4ccc384a38d7)



#### 2. [[⬆]](#toc) <a name='availability'>Попасть в систему без пароля несколькими способами</a>

#####  Способ 1. init=/bin/bash
Для получения доступа необходимо открыть консоль, выполнить reboot и при выборе ядра для загрузки нажать `e` - в данном контексте edit. Попадаем в окно, где мы можем изменить параметры загрузки. Находим строку linux, находим запись `ro` и меняем ее на `rw` и в конце строки дописываем `init=/bin/bash`. Далее жмем `сtrl-x` для загрузки системы. Также можно `ro` на этом шаге на менять, а загрузится в режиме Read-Only, а уже после загрузки системы можно ее перемонтировать командой `mount -o remount,rw /`.  

![image](https://github.com/user-attachments/assets/0574e83c-526c-4cef-aaa2-4640f9d699fa)


#####  Способ 2. Recovery mode
В меню загрузчика на первом уровне выбрать второй пункт (Advanced options…), далее загрузить пункт меню с указанием recovery mode в названии.  Получим меню режима восстановления.

![image](https://github.com/user-attachments/assets/1eedee3c-86aa-47cf-ac5a-77b820f6f0a3)

В этом меню сначала включаем поддержку сети (network) для того, чтобы файловая система перемонтировалась в режим read/write (либо это можно сделать вручную). Далее выбираем пункт root и попадаем в консоль с пользователем root. Если вы ранее устанавливали пароль для пользователя root (по умолчанию его нет), то необходимо его ввести.  В этой консоли можно производить любые манипуляции с системой.

#####  Способ 3. Консоль GRUB
Чтобы попасть в консоль GRUB, нужно нажать клавишу `C` во время отображения меню загрузки.

![image](https://github.com/user-attachments/assets/2338bab3-e4fe-4545-b5c4-9ceb4724f036)

Подробнее - https://www.alexgur.ru/articles/2275/




#### 3. [[⬆]](#toc) <a name='availability'>Установить систему с LVM, после чего переименовать VG</a>

![image](https://github.com/user-attachments/assets/8c9e9f17-0233-4289-b78d-8b05094dd06d)

Нас интересует вторая строка с именем Volume Group. Приступим к переименованию:
![image](https://github.com/user-attachments/assets/0446f614-ec8b-4b84-84ce-bba58873351b)

Далее правим `/boot/grub/grub.cfg`. Везде заменяем старое название VG на новое (в файле дефис меняется на два дефиса ubuntu--vg ubuntu--otus).
После чего можем перезагружаться и, если все сделано правильно, успешно грузимся с новым именем Volume Group и проверяем:

```
sudo nano /boot/grub/grub.cfg
```
![image](https://github.com/user-attachments/assets/e95d35c3-a1ad-4f1b-902e-e856d0496be7)




#### 4. [[⬆]](#toc) <a name='recommended_sources'>Дополнительные источники</a>

1. [GRUB - загрузчик системы](https://help.ubuntu.ru/wiki/grub)
2. [GRUB консоль. Запускаем Linux](https://www.alexgur.ru/articles/2275/)
3. [Настройка загрузчика Grub](https://losst.pro/nastrojka-zagruzchika-grub)
4. [https://forum.ubuntu.ru/index.php?topic=74165.0](https://forum.ubuntu.ru/index.php?topic=74165.0) 
5. Весь Linux Для тех, кто хочет стать профессионалом, стр.311
