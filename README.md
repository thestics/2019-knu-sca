# KNU 2019 SCA

## Шаблон

Для решения задач можно использовать [шаблон](tasks/template.ipynb) от [Jupyter](https://jupyter.org/). Можно создать свой если хочется.

Для запуска используйте:
```sh
> jupyter notebook
[I 00:00:00.000 NotebookApp] Serving notebooks from local directory: /XXXXXXXXXXXXXXXXXXXX
[I 00:00:00.000 NotebookApp] 0 active kernels
[I 00:00:00.000 NotebookApp] The Jupyter Notebook is running at:
[I 00:00:00.000 NotebookApp] http://localhost:8888/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
[I 00:00:00.000 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).

```

## Задача 1

С помощью CPA необходимо достать ключ из [трейсов](tasks/task1). На трейсах AES 128, шифрование, *первые* раунды. Можно использовать [коэффициент корреляции Пирсона](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient).

Описание трейсов:
* `textin.npy`. открытый текст.
* `textout.npy`. шифротекст.
* `traces.npy`. сам сигнал.

Для загрузки `.npy`  используйте  [numpy.load](https://docs.scipy.org/doc/numpy/reference/generated/numpy.load.html).

## Задача 2

С помощью CPA необходимо достать ключ из [трейсов](tasks/task2). На трейсах AES 128, шифрование, *последние* раунды. Задача похожа на первую, но нужно убрать шум и оставить полезную часть сигнала и изменить модель утечки, чтобы она соответствовала последнему раунду. Захват сигнала производился с частотой 32Mhz, шум в пределах 28.5-32Mhz, скорость контроллера 8Mhz.

Для фильтрации можно использовать, как прямое и обратное преобразование Фурье, так и полосный фильтр [scipy.butter](https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.signal.butter.html) и [scipy.lfilter](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html).

## Задача 3

С помощью CPA необходимо достать ключ из [трейсов](tasks/task3). На трейсах AES 128, шифрование, *первые* раунды. Задача похожа на первую. Необходимо выровнять трейсы относительно друг друга.

Для выравнивания можно использовать [SAD](https://en.wikipedia.org/wiki/Sum_of_absolute_differences):
1. Первый трейс выбираем референсным.
1. Обозначаем _окно_ `window_start, window_end`, паттерн которого будем искать в трейсах.
1. Обозначаем _промежуток выравнивания_ `range_start, range_end)` для всех остальных трейсов, в пределах которого будем двигать _окно_.
1. Для каждого из трейса, кроме референсного:
   1. Ставим _окно_ в начало _промежутка выравнивания_
   1. Вычитаем сигнал в _окне_ референсного трейса с _окном_ текущего трейса.
   1. Вычисляем сумму значений сигнала, полученного в результате вычитания
   1. Сохраняем полученную сумму
   1. Сдвигаем _окно_ в текущем трейсе на одну позицию влево
   1. Повторяем подпункты ii-vi, пока конец _окна_ не достигнет конца _промежутка выравнивания_
   1. в месте, где сумма была минимальна, сигнал был наиболее похож на референсный, т.е. на это значение нужно сдвинуть текущий трейс