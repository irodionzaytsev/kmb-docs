# Computer Forensic
---
### Введение
**Forensic (Computer forensic)** - прикладная наука о раскрытии инцидентов, связанных с компьютерной информацией, исследовании цифровых доказательств, методах поиска, получения и закрепления таких доказательств. Форензика является подразделом криминалистики, является неотъемлемой частью в сфере ИБ.

Виды инцидентов:
- утечка конфиденциальной информации 
- неправомерный доступ к информации
- удаление информации
- компрометация информации
- саботаж
- мошенничество с помощью ИТ систем
- использование активов компании в личных целях
- внешние атаки: DoS, DDoS, фишинг, перехват и подмена трафика
- размещение конфиденциальной/провокационной информации в сети Интернет
- взлом
- вирусные атаки
---
---
### Forensic в CTF

В CTF *forensic* является одной из сложных категорий заданий, сравнимой с PWN. Эта категория охватывает довольно обширные категории знаний:
- Программирование
- ОС (Windows, Unix, ~~BolgenOS~~)
- ФС (FAT, NTFS, Ext, etc.)
- Специфика типов файлов (JPEG, ELF, WAV, etc.)
- Сети (как минимум стек протоколов TCP/IP)
- Криптография
- Стеганография
- RE
- OSINT (Open Source INTelligence)

Виды задач, встречающиеся в тасках CTF:
- Восстановление данных (в том числе и удаленных)
- Анализ логов (журналы аудита, лог-файлы программ)
- Анализ сетевого трафика
- Поиск информации из открытых источников

Задачи могут перемежаться между собой, а также быть усложнены другими категориями знаний (криптография, RE, вирусология).

Каких-либо универсальных методов решения тасков категории *forensic* нет. Никогда не знаешь, что тебе за инцидент попадется и как тебе с ним ~~париться~~ справляться. Можно лишь выработать стратегию решения, например:
- Что за ~~фигню~~ объект мы имеем?
- Какие особенности имеет тип объекта?
- Какие отличия имеет объект от эталонного типа объекта?
- Какие методы решения существуют?

Эту стратегию следует зациклить до тех пор, пока задача не будет решена. Например, в исходных данных мы имеем дамп сетевого трафика. Проанализировав его, мы определили, что там передавались какие-то данные. После успешного (или не очень) извлечения мы получаем новый объект. Мы снова анализируем, что это за объект, какие он имеет особенности, что с ним не так и что с этим дальше делать.

Инструменты для решения задач *forensic*:
 - Сетевые утилиты (*Wireshark, Tshark, Scapy*)
 - Файловые утилиты (*file, head, hex-редакторы*)
 - Утилиты для работы с ФС (*TSK, Foremost, Autopsy*)
 - Крипто-утилиты (*Cryptool*) 
 - Графические редакторы (*GIMP, PS*)
 - Аудиоредакторы (*Audacity, AU*)
 - Языки программирования (*Python, C, ~~Brainfuck~~*)
---
---
### Пример задания

Один из тасков на соревнованиях PlaidCTF 2015 - [***Unknown***](http://play.plaidctf.com/files/unknown_2348c21020c876be4ae7d9eb19f8500a).

Из исходных данных - файл непонятного содержания. Первое что приходит в голову - отдать файл утилите *file* (КЭП рядом). Результат:

>```sh
>such@n00b:/tmp$ file unknown_2348c21020c876be4ae7d9eb19f8500a 
>unknown_2348c21020c876be4ae7d9eb19f8500a: LaTeX document, ASCII text
>```

Пробуем открыть редаткором LaTeXа - ничего вразумительного. Посмотрим любым текстовым редактором содержание файла, увидим следующее:

>```
>\begindata{raster,1}
>2 0 65536 65536 0 0 640 400
>bits 1 640 400
>5a5c0b2f620b86f56c220475ab062
>```

Погуглим значение первой строки (Нечто похожее на хедер какого-то файла). Первая же [ссылка](http://www.fileformat.info/format/cmu/spec/c4cfb8404a304ea687b344485c445eb2/view.htm) дает нам вполне вразумительную информацию о файле.

> Format of ATK raster images 
>
> The raster data object writes a standard ATK data stream beginning with 
> a \begindata line and ending with a \enddata line.  Between these comes 
> a header and possibly an image body. 
>
> The first line of the header looks like this: 
>
> 2 0 65536 65536 0 0 484 603 

Из статьи делаем вывод, что перед нами - растровое изображение какой-то ~~фигни~~ Andrew User Interface System. Расширение файла - **.CMU**. Далее опять обращаемся к гуглу за тем, что же нам может открыть данный тип изображений, на что получаем ответ - *XnView*. Качаем программу, ставим, переименовываем файл в *unknown.CMU*, открываем, получаем результат - **flag{l0l_CMU_da_b3s}**

---

*Автор: Гилёв С.В. aka PaHUEllo*