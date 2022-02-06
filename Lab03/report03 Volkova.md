---
# Front matter
title: "Отчёт по лабораторной работе №3"
subtitle: "Шифрование гаммированием"
author: "Волкова Дарья Александровна НПМмд-02-21"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Pdf output format
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
### Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Изучение алгоритма шифрования гаммированием конечной гаммой, а также его программная реализация.

# Теоретические сведения

Частным случаем многоалфавитной подстановки является гаммирование. В этом способе шифрование выполняется путем сложения символов исходного текста и ключа по модулю, равному числу букв в алфавите. Если в исходном алфавите, например, 33 символа, то сложение производится по модулю 33. Такой процесс сложения исходного текста и ключа называется в криптографии наложением гаммы.

Под гаммированием понимают наложение на открытые данные по определенному закону гаммы шифра (двоичного числа, сформированного на основе генератора случайных чисел).

Гамма шифра – псевдослучайная последовательность, вырабатываемая по определенному алгоритму, используемая для шифровки открытых данных и дешифровки шифротекста.

Принцип шифрования заключается в формировании генератором псевдослучайных чисел (ГПСЧ) гаммы шифра и наложении этой гаммы на открытые данные обратимым образом, например, путем сложения по модулю два. Процесс дешифрования данных сводится к повторной генерации гаммы шифра и наложении гаммы на зашифрованные данные. Ключом шифрования в данном случае является начальное состояние генератора псевдослучайных чисел. При одном и том же начальном состоянии ГПСЧ будет формировать одни и те же псевдослучайные последовательности.

Перед шифрованием открытые данные обычно разбивают на блоки одинаковой длины, например, по 64 бита. Гамма шифра также вырабатывается в виде последовательности блоков той же длины.

Стойкость шифрования методом гаммирования определяется главным образом свойствами гаммы – длиной периода и равномерностью статистических характеристик. Последнее свойство обеспечивает отсутствие закономерностей в появлении различных символов в пределах периода. Полученный зашифрованный текст является достаточно трудным для раскрытия. По сути дела гамма шифра должна изменяться случайным образом для каждого шифруемого блока.

Обычно разделяют две разновидности гаммирования – с конечной и бесконечной гаммами. При хороших статистических свойствах гаммы стойкость шифрования определяется только длиной периода гаммы. При этом, если длина периода гаммы превышает длину шифруемого текста, то такой шифр теоретически является абсолютно стойким, т.е. его нельзя вскрыть при помощи статистической обработки зашифрованного текста, а можно раскрыть только прямым перебором. Криптостойкость в этом случае определяется размером ключа.

# Выполнение работы

## Реализация алгоритмов на языке Python

```
# Шифрование гаммированием конечной гаммой

message = input('Введите строку: ').lower()
gamma = str(input('Введите гамму: ')).lower()

alphabet = 'абвгдежзийклмнопрстуфхцчшщъыьэюя'

n = len(gamma)
m=len(message)

k = (m % n) # Количество символов которые нужно дополнить
gamma_len = '' + gamma * (m // n) + gamma[:k]
print(message, gamma_len, sep='\n')

c=[]

for i in range(m):
    c.append(alphabet.find(message[i])+(alphabet.find(gamma_len[i])+1)%33)

encrypted = ''

for i in range(len(c)):
    encrypted+=alphabet[c[i]]

print('Криптограмма:', encrypted)
```

## Контрольный пример

![Пример работы алгоритма](https://github.com/volkk3/Mathematical-foundations-of-information-protection-and-information-security/blob/main/Lab03/screen/png03.PNG){ #fig:001 width=70% height=70%}


# Выводы

В ходе выполнения работы мне удалось изучить задачу разложения на множители и p-алгоритм Полларда, а также реализовать данный алгоритм программно на языке Python.


# Список литературы{.unnumbered}

1. [Алгоритмы тестирования на простоту и факторизации](https://habr.com/ru/post/521876/)
2. [P-метод Полларда](https://ru.bmstu.wiki/P-метод_Полларда)
