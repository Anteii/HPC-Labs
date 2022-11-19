
# Отчет по третьей лабораторной работе (STFT with FFTW and cuFFT)

## Содержание отчета

* [Описание работы](#описание-работы)
* [Особенности реализации](#особенности-реализации)
* [Результаты](#результаты)

## Описание работы
Данная лабораторная работа выполнена в среде google colab, поскольку она предоставлет доступ к виртуальной линукс машине с видеоадаптером nvidia (cuda-compatible, в моем случае это была nvidia tesla T4) и дает возможность компилировать и исполнять C++/Cuda программы. Программы Cuda компилировались с nvcc, FFTW - g++. Вывод программы перенаправляется в вывод ячейки.

Ноутбук состоит из 6 разделов:

- Setup (установка FFTW, cudaDeviceQuery, EasyBMP и тестовой музыки)
- WAV Structures (определение структуры WAV файла)
- CuFFT (cuFFT программа)
- FFTW (FFTW программа)
- Visualization (построение графиков и других визуализаций полученных результатов)
- Stash (Не исопльзуемый код)

## Особенности реализации

Особенности реализации:

- В cuFFT реализации все сэмплы хранятся на девайсе и по мере необходимости копируются в буфер (на девайсе/хосте)
- FFTW программа компилировалась с флагом -O2 (тестировались разные ключи, но не дали существенного прироста)
- FFTW и cuFFT планы создаются один раз
- Настраиваемый размер окна и шаг
- Каждый эксперимент выполнялся несколько раз с последующим усреднением времени

## Результаты
В ходе работы Я ознакомился с преобразованием Фурье, STFT, библиотеками cuFFT и FFTW, установкой и сборкой библиотек в UNIX системе.
Также были произведены замеры времени работы программ (для программы с cuFFT был произведен раздельный замер коммуникаций и вычислений).

<p align="center">
  Время, затраченное на пересылку данных(cuFFT)<br>
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab3/resources/cufft_transfer_barplot.jpg"/>
</p>

<p align="center">
  Время, затраченное на вычисления (cuFFT)<br>
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab3/resources/cufft_calc_barplot.jpg"/>
</p>

<p align="center">
  Общее время, затраченное на вычисление спектрограммы (cuFFT)<br>
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab3/resources/cufft_barplot.jpg"/>
</p>

<p align="center">
  Время работы FFTW программы<br>
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab3/resources/fftw_barplot.jpg"/>
</p>


<p align="center">
  Ускорение, получаемое при переходе от FFTW к cuFFT<br>
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab3/resources/speed_up.jpg"/>
</p>
