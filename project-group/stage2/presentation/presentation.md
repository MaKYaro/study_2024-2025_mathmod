---
## Front matter
lang: ru-RU
title: Этап 2
subtitle: "Алгоритм решения задачи"
author:
  - Канева Екатерина
  - Клюкин Михаил
  - Ланцова Яна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 апреля 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - \usepackage{fontspec}
 - \usepackage{polyglossia}
 - \setmainlanguage{russian}
 - \setotherlanguage{english}
 - \newfontfamily\cyrillicfont{Arial}
 - \newfontfamily\cyrillicfontsf{Arial}
 - \newfontfamily\cyrillicfonttt{Arial}
 - \setmainfont{Arial}
 - \setsansfont{Arial}
 
---

# Информация

## Состав исследовательской команды

Студенты группы НФИбд-02-22:

- Канева Екатерина
- Клюкин Михаил
- Ланцова Яна

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

:::
::::::::::::::

## Цель работы

Описать алгоритмы для моделирования колебания цепочки атомов.

## Задание

1. Описать алгоритм для моделирования гармонических колебаний.
2. Описать алгоритм для моделирования ангармонических колебаний.

# Выполнение лабораторной работы

# Алгоритм для гармонических колебаний

## Алгоритм для гармонических колебаний

$$
x_i = i dx_i​ = i_d
$$

## Исходные уравнения

$$
m \dfrac{d^2 y_i}{dt^2} = k(y_{i+1} - 2y_i + y_{i-1}), \quad i = 1, \dots, N.
$$

## Задание параметров

- $N$ -- количество подвижных частиц
- $m$ -- масса каждой частицы
- $k$ -- жесткость пружины между частицами
- $T$ -- общее время моделирования
- $\Delta t$ -- шаг по времени

- $\Delta d$ -- расстояние между частицами (может быть единичным)

## Инициализация массивов

- $y[i]$ -- смещения частиц
- $v[i]$ -- скорости частиц
- $a[i]$ -- ускорения частиц

## Основной цикл моделирования

$$
a_i = \dfrac{k}{m}(y_{i+1} - 2y_i + y_{i−1})
$$

$$
v_i (t + \Delta t) = v_i(t) + a_i \Delta t
$$

$$
y_i (t + \Delta t) = y_i(t) + v_i(t + \Delta t) \Delta t
$$

# Алгоритм для ангармонических колебаний

## Алгоритм для ангармонических колебаний

$$
m \dfrac{d^2 y_i}{dt^2} ​​= k(y_{i+1}​ − 2y_i​ + y_{i−1}​) + \alpha[(y_{i+1}​ − y_i​)^3 + (y_{i−1}​ − y_i​)^3]
$$

## Задание параметров

- $N$ — количество подвижных частиц
- $m$ — масса частицы
- $k$ — жесткость пружины
- $\alpha$ — коэффициент ангармоничности
- $\Delta t$ — шаг по времени
- $T$ — общее время моделирования

## Инициализация массивов

- $y[i]$ — смещения
- $v[i]$ — скорости
- $a[i]$ — ускорения
- индексы $\quad i = 1, \dots, N$ (фиктивные граничные условия)

## Цикл по времени

$$
a_i = \dfrac {1}{m} [k(y_{i+1} - 2y_i + y_{i-1}) + \alpha ((y_{i+1} - y_i)^3) + (y_{i-1} - y_i)^3]
$$

$$
v_i (t + \Delta t) = v_i(t) + a_i \Delta t
$$

$$
y_i (t + \Delta t) = y_i(t) + v_i (t + \Delta t) \Delta t
$$

## Выводы

В ходе выполнения второй части группового проекта мы описали алгортмы для моделирования колебания цепочки атомов.

