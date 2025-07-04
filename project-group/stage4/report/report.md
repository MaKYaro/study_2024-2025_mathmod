---
## Front matter
title: Этап 4
subtitle: "Результаты проекта"
author: 

  - Канева Екатерина
  - Клюкин Михаил
  - Ланцова Яна

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
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
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

**Актуальность**

Все вещества состоят из атомов, которые постоянно колеблются. Изучение этих колебаний помогает нам понять, как материалы ведут себя при разных температурах. Особенно важно понимать, как колебания приводят к тепловому равновесию. Исследование цепочек атомов, связанных пружинками, это простая модель, чтобы понять, как возникают колебания в кристаллах. Эта модель помогает объяснить, почему некоторые классические законы физики работают только при высоких температурах. Понимание колебаний важно для создания новых материалов с нужными свойствами, например, для электроники или термоизоляции.

**Цель работы**

Исследовать закономерности колебаний в простейшей одномерной цепочке атомов, связанных между собой.

**Объект и предмет исследования**

1. Изучение условий для установления равновесия 
2. Изучение условий для приближения к равновесию
3. Изучение явлений в простейшем одномерном случае

**Задачи**

1. Построить модель цепочки из N частиц.
2. Описать алгоритм для моделирования гармонических и ангармонических колебаний.
3. Реализовать программу для моделирования гармонических и ангармонических колебаний.

**Материалы и методы**

Язык программирования Julia

- Plots.jl
- LinearAlgebra
- FFTW

# Теоретическое описание задачи 

## Гармоническая цепочка

Если начально пружины не деформированы, то положение равновесия i-й частицы — это $x_i = id$. Вводятся малые смещения $y_i$ от положения равновесия $y_i$ (считаем, что $y_i$ много меньше $d$). Тогда силы, действующие на $i$-ю частицу, записываются как:

$$
F_i = k(y_{i+1} - y_i) - k(y_i - y_{i-1}) = k(y_{i+1} - 2y_i + y_{i-1}).
$$

Соответственно, уравнение движения имеет вид:

$$
m \dfrac{d^2 y_i}{dt^2} = k(y_{i+1} - 2y_i + y_{i-1}), \quad i = 1, \dots, N.
$$

### Полная энергия системы

Полная энергия системы учитывает как кинетическую, так и потенциальную энергию:

$$
U = \dfrac{m}{2} \sum_{i=1}^{N} \left( \frac{d y_i}{dt} \right)^2 + \dfrac{k}{2} \sum_{i=1}^{N+1} (y_i - y_{i-1})^2.
$$

### Решение уравнения

Физически обоснованным решением является форма стоячей волны:

$$
y_i = \left( A \cos (p x_i) + B \sin (p x_i) \right) \cos (\omega t).
$$

Граничные условия $y_0 = 0$ и $y_{N+1} = 0$ приводят к ограничению на $p$:

$$
\sin (p (N+1)d) = 0.
$$

Это выполняется при значениях:

$$
p_l = \dfrac{l \pi}{(N+1)d}, \quad l = 1, \dots, N.
$$

## Ангармоническая цепочка

Для реальных пружин линейное выражение для возвращающей силы
$F = −kx$ верно только для малых деформаций. При больших сжатиях
пружины сила обычно больше, а при больших растяжениях меньше, чем $kx$. Такую зависимость можно получить, добавив еще одно слагаемое к силе:

$$
 F = -kx \left(1 - \dfrac{\alpha x}{d} \right).
$$

Здесь $\alpha$ — безразмерный коэффициент. В этом случае выражение для энергии будет выглядеть так:

$$
 U = \dfrac{m}{2} \sum_{i=1}^{N} \left( \frac{d y_i}{dt} \right)^2 + \dfrac{k}{2} \sum_{i=1}^{N+1} \left(y_i - y_{i-1}\right)^2 - \dfrac{k \alpha}{3d} \sum_{i=1}^{N+1} \left(y_i - y_{i-1}\right)^3.
$$

# Программная реализация

```Julia
using Plots
using LinearAlgebra
using FFTW
using Dates

# Гармонические колебания
function harmonic_chain_simulation(;
    N=20,          # Количество частиц
    m=1.0,         # Масса частицы
    k=1.0,         # Жёсткость пружины
    alpha=0.0,         # Коэффициент ангармоничности (0 для гармонического случая)
    T=100.0,       # Общее время моделирования
    dt=0.01,       # Шаг по времени
    dd=1.0,        # Расстояние между частицами
    initial_displacement=0.1,  # Амплитуда начального возмущения
    save_every=10  # Сохранять состояние каждые save_every шагов
)
    # Инициализация массивов (включая граничные условия)
    y = zeros(N+2)  # Смещения (y[1] и y[N+2] - граничные условия)
    v = zeros(N+2)  # Скорости
    a = zeros(N+2)  # Ускорения
    
    # Начальные условия - синусоидальное возмущение
    for i in 2:N+1
        y[i] = initial_displacement * sin(pi*(i-1)/N)
    end
    
    # Массивы для сохранения результатов
    times = Float64[]
    positions = Vector{Float64}[]
    velocities = Vector{Float64}[]
    
    # Основной цикл моделирования
    for t in 0:dt:T
        # Вычисление ускорений для внутренних частиц
        for i in 2:N+1
            dy_prev = y[i] - y[i-1]
            dy_next = y[i+1] - y[i]
            
            # Гармоническая часть силы
            F_harmonic = k * (y[i+1] - 2*y[i] + y[i-1])
            
            # Ангармоническая часть силы (если alpha ≠ 0)
            F_anharmonic = alpha * (dy_next^3 + dy_prev^3)
            
            a[i] = (F_harmonic + F_anharmonic) / m
        end
        
        # Обновление скоростей и смещений (метод Верле)
        for i in 2:N+1
            v[i] += a[i] * dt
            y[i] += v[i] * dt
        end
        
        # Применение граничных условий
        y[1] = 0.0
        y[N+2] = 0.0
        
        # Сохранение состояния (не на каждом шаге для экономии памяти)
        if mod(round(t/dt), save_every) == 0
            push!(times, t)
            push!(positions, copy(y[2:N+1]))  # Исключаем граничные точки
            push!(velocities, copy(v[2:N+1]))
        end
    end
    
    return times, positions, velocities
end
```

# Результаты

Выполнив моделирование, получим колебания гармонического осциллятора (рис. [-@fig:001]).

![Колебания гармонического осциллятора](image/harmonic_2025-05-02_09-58-07.gif){#fig:001 width=70%}

Также смоделируем колебания ангармонического осциллятора при коэффициенте ангармоничности $\alpha = 50$ (рис. [-@fig:002]).

![Колебания ангармонического осциллятора](image/anharmonic_2025-05-02_09-58-15.gif){#fig:002 width=70%}

# Выводы

- Построили модель цепочки из N частиц.
- Описали алгоритм для моделирования гармонических и ангармонических колебаний.
- Реализовали программу для моделирования гармонических и ангармонических колебаний.

