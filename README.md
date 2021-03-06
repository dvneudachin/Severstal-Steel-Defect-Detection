# Северсталь: обнаружение дефектов стали.

**Описание**: https://www.kaggle.com/c/severstal-steel-defect-detection/overview/description

**Цель** - предсказать местоположение и тип дефектов, обнаруженных при производстве стали, используя предоставленные изображения. Названия изображений имеют уникальный идентификатор, и задача состоит в том, чтобы сегментировать каждое изображение и классифицировать дефекты в наборе тестовых данных.

**Подходы к решению:**

-  Импортируем и ознакамливаемся с данными;


-  Видим, что даны обучающая выборка, состоящая из 12568 уникальных изображений и тестовая выборка, состоящая из 1801 изображений. Так же предлагается текстовый файл, с информацией о дефекте на каждом изображении;


-  В ходе анализа данных видим, что распределение количества изображений с дефектами отличается незначительно: 5902 изображения без дефектов и 6666 изображений с дефектами. Это хорошо сбалансированная проблема для задачи бинарной классификации;

 
-  В исходных данных из обучающей выборки есть дисбаланс в распределении количества изображений по классам. Количество изображений с дефектом второго класса очень мало, а количество изображений с дефектом третьего класса очень велико. Первый и четвертый классы несколько сбалансированы;

 
-  Взглянем на некоторые изображения каждого класса. Эту визуализацию можно легко сделать, маскируя заданные кодированные пиксели на изображениях данных из тренировочной выборки. Повреждения первого класса наименее выражены и почти аналогичны не дефектным изображениям. Можно заметить, что они похожи на дефекты первого класса, и между ними довольно сложно провести классификацию. Также дефекты третьего класса гораздо обширнее и не похожи на первый и второй классы;


-  Задачу можно разделить на 2 этапа: 1) Создадим простой бинарный классификатор, который будет предсказывать, есть ли хоть один дефект на изображении или нет. Это позволит отсеять изображения без дефектов для следующего этапа. 2) Реализуем сеть на базе Unet для предсказания маски дефекта. Архитектура сети представляет собой полносвязную свёрточную сеть, модифицированную так, чтобы она могла работать с меньшим количеством примеров (обучающих образов) и делала более точную сегментацию. Сеть содержит сжимающий путь и расширяющий путь, поэтому архитектура похожа на букву U, что и отражено в названии. На каждом шаге мы удваиваем количество каналов признаков. Изначально создана и применена для сегментации биомедицинских изображений: 
https://www.kaggle.com/jesperdramsch/intro-chest-xray-dicom-viz-u-nets-full-data#Vanilla-Unet


-  В качестве оптимизатора модели использован классический оптимизатор Adam, который обеспечивает автоматическую настройку адаптивной скорости обучения на основе их детального изучения влияния дисперсии и импульса во время обучения;


-  Применена BCE (Binary Cross-Entropy) функция потерь для данной модели. Эта функция независима от каждого векторного компонента, что позволяет использовать ее для классификации по нескольким классам;


-  Обучение проводилось на 25 эпохах с оценкой показателей loss (функции потерь);


-  Хотя значение потерь продолжает уменьшаться, результаты проверки не улучшаются. 
     Достигнут Dice coefficient (коэффициент Сёренсена) = 0.4147.



