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

С помощью CPA необходимо достать ключ из [трейсов](tasks/task1). На трейсах AES 128, шифрование, первые раунды. Можно использовать [коэффициент корреляции Пирсона](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient).

Описание трейсов:
* `textin.npy`. открытый текст.
* `textout.npy`. шифротекст.
* `traces.npy`. сам сигнал.

Для загрузки `.npy`  используйте  [numpy.load](https://docs.scipy.org/doc/numpy/reference/generated/numpy.load.html).