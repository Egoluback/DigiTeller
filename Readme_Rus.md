# DigiTeller

Технологии искусственного интеллекта набирают все бо́льшую популярность в креативных индустриях: с их помощью пишут музыку, создают уникальные арт-объекты и многое другое. 
Мы хотим предложить вам создать решение, которое поможет в создании видеоистории. Подобного рода алгоритмы значительно облегчают или даже полностью автоматизируют трудоёмкий процесс создания новых историй и их визуализации.  


Мы предоставляем Baseline, состоящий из 5 ноутбуков Google Colab:

[Part 1](https://colab.research.google.com/drive/1tnWMZ26NygRQS1XpHDDBPBiPJvoUFcRN?usp=sharing)

[Part 2](https://colab.research.google.com/drive/1CTw058KVOgbavUacXSa0fj1v-OgJywwJ?usp=sharing)

[Part 3](https://colab.research.google.com/drive/1Kt0Mb3_6O6RjUzPhYaK_3xeRANYtlxAo?usp=sharing)

[Part 3a](https://colab.research.google.com/drive/1W2NL8vW9sij2xqNHclJqkw9FaVFXBioR?usp=sharing)

[Part 4](https://colab.research.google.com/drive/13sOtHgokbsDnC9l69VtQRDlRcI5Zau3Z?usp=sharing)

Задача состоит из 5-ти больших блоков, подробнее о них рассказано ниже.

## Блок 1. Text Generation
Огромные языковые модели (вроде GPT-3) все больше удивляют нас своими возможностями. И хотя пока доверие к ним со стороны бизнеса недостаточно для того, чтобы представить их своим клиентам, эти модели демонстрируют те зачатки разума, которые позволят ускорить развитие автоматизации и возможностей «умных» компьютерных систем. Давайте снимем ауру таинственности с GPT-3 и узнаем, как она обучается и как работает.
Обученная языковая модель генерирует текст. Мы можем также отправить на вход модели какой-то текст и посмотреть, как изменится выход. Последний генерируется из того, что модель «выучила» во время периода обучения путем анализа больших объемов текста.
Обучение – это процесс передачи модели большого количества текста. Для GPT-3 этот процесс завершен и все эксперименты, которые вы сможете увидеть, проводятся на уже обученной модели. Было подсчитано, что обучение должно было занять 355 GPU-лет (355 лет обучения на одной видеокарте) и стоить 4.6 миллиона долларов

![1-1](https://user-images.githubusercontent.com/57997673/119852987-75b74f00-bf18-11eb-9835-2a3708f94252.png)
На ввод модели мы подаем один пример (отображаем только признаки) и просим ее предсказать следующее слово предложения.
Поначалу предсказания модели будут ошибочны. Мы подсчитываем ошибку в предсказании и обновляем модель до тех пор, пока предсказания не улучшатся.
И так несколько миллионов раз.

![1-2](https://user-images.githubusercontent.com/57997673/119853198-a6978400-bf18-11eb-8b57-a60aef2f360f.png)
Важные вычисления GPT-3 происходят внутри стека из 96 слоев декодера Трансформера.
Видите все эти слои? Это и есть та самая «глубина» «глубокого обучения» (deep learning).
У каждого слоя есть свои 1.8 миллиардов параметров для вычислений. Здесь и происходит вся «магия». Верхнеуровнево этот процесс можно изобразить следующим образом:

![1-3](https://user-images.githubusercontent.com/57997673/119853333-c75fd980-bf18-11eb-9b76-d8bc694b9d4b.png)

## Блок 2. Text2Speech model
Применение конкатенативного TTS ограничено из-за больших требований к данным и времени разработки. Поэтому был разработан статистический метод, исследующий саму природу данных. Он генерирует речь с помощью комбинирования таких параметров, как частота, спектр амплитуд и т.д.
Параметрический синтез состоит из следущих этапов.
Сначала из текста извлекаются лингвистические признаки, например, фонемы, продолжительность и т.д.
Затем для вокодера (системы, генерирующей wave-формы) извлекаются признаки, которые представляют соответствующий речевой сигнал: кепстр, частота, линейная спектрограмма, мел-спектрограмма.
Эти, настроенные вручную, параметры наряду с лингвистическими особенностями передаются в модель вокодера, а тот выполняет множество сложных преобразований для генерирования звуковой волны. При этом вокодер оценивает параметры речи, такие как фаза, просодия, интонация и другие.
Если мы можем аппроксимировать параметры, которые определяют речь на каждой её единице, тогда мы сможем создать параметрическую модель. Параметрический синтез требует значительно меньше данных и тяжелой работы, нежели конкатенативные системы.

![2-1](https://user-images.githubusercontent.com/57997673/119853730-1f96db80-bf19-11eb-9cb7-2796d6a41c54.png)

## Блок 3. Music Generation
Music Transformer - нейронная сеть от OpenAI, основанная на внимании, которая может генерировать музыку с улучшенной долгосрочной когерентностью.
В модели используется представление на основе событий, которое позволяет нам напрямую генерировать выразительные выступления. Всего в модели 388 событий: 128 событий включения нот различных высот, 128 событий выключения нот различных высот, 100 событий перехода на следующий временной промежуток и 32 события изменения скорости.
В модели используется механизм относительного внимания, который явно модулирует внимание в зависимости от того, насколько далеко друг от друга находятся два токена, модель может больше сосредоточиться на реляционных функциях. Относительное самовнимание также позволяет модели обобщать за пределами длины обучающих примеров, что невозможно с исходной моделью Transformer.

![2-2](https://user-images.githubusercontent.com/57997673/119854082-70a6cf80-bf19-11eb-8a2a-1aae4cc67be0.png)


## Бонус. Генерация музыки нейросетью Jukebox

Jukebox - нейронная сеть от OpenAI, которая генерирует музыку, в том числе элементарное пение, в виде необработанного звука в различных жанрах и стилях исполнителей.
Модель автоэнкодера Jukebox сжимает звук в дискретное пространство, используя подход, основанный на квантовании, который называется VQ-VAE. В Jukebox используется модернизированный VQ-VAE.
Используется три уровня в VQ-VAE, показанном ниже, которые сжимают необработанный звук 44 кГц в 8x, 32x и 128x, соответственно. Понижающая дискретизация теряет большую часть деталей звука и звучит заметно шумно по мере спуска дальше по уровням. Однако она сохраняет важную информацию о высоте, тембре и громкости звука.
### Сжатие

![2-3](https://user-images.githubusercontent.com/57997673/119854417-b82d5b80-bf19-11eb-9768-fec125905aac.png)

После того, как все априоры обучены, музыка генерируется на верхнем уровне, а затем повышается дискретизация с помощью апсэмплеров и декодируется обратно в необработанное звуковое пространство с помощью декодера VQ-VAE для семплирования новых песен.

### Генерация

![2-4](https://user-images.githubusercontent.com/57997673/119854500-c9766800-bf19-11eb-9969-8c88a5073b0c.png)

Чтобы обратить внимание на тексты песен, добавляется кодировщик для создания представления текстов песен и добавляются уровни внимания, которые используют запросы от музыкального декодера для обработки ключей и значений от кодировщика текстов. После обучения модель учится более точному выравниванию текста.

## Блок 4. Генерация клипа, CLIP + BIGGAN

CLIP (Contrastive Language – Image Pre-training) основан на большом объеме работы по zero-shot transfer, контролю естественного языка и мультимодальному обучению. Идея обучения с zero-data возникла в 80хх, но до недавнего времени в основном изучалась в области компьютерного зрения как способ обобщения на категории невидимых объектов.
Критически важным было использование естественного языка в качестве гибкого пространства для прогнозирования, позволяющего обобщать и передавать . В 2013 году Richer Socher и соавторы из Stanford разработали доказательство концепции, обучив модель на CIFAR-10 делать предсказания в пространстве встраивания векторов слов, и показали, что эта модель может предсказывать два невидимых класса. В том же году DeVISE12 масштабировал этот подход и продемонстрировал, что можно точно настроить модель ImageNet, чтобы ее можно было обобщить для правильного прогнозирования объектов за пределами исходной 1000 обучающей выборки.
Подход

Мы показываем, что масштабирования простой задачи предварительного обучения достаточно для достижения конкурентоспособной производительности с нулевым выстрелом на большом количестве наборов данных классификации изображений. В нашем методе используется широко доступный источник контроля: текст в сочетании с изображениями, найденными в Интернете. Эти данные используются для создания следующей задачи обучения прокси для CLIP: по заданному изображению предсказать, какой из 32 768 произвольно выбранных фрагментов текста фактически был связан с ним в нашем наборе данных.
Наша интуиция подсказывает, что для решения этой задачи модели CLIP должны научиться распознавать широкий спектр визуальных концепций в изображениях и связывать их со своими именами. В результате модели CLIP можно применять к почти произвольным задачам визуальной классификации. Например, если задачей набора данных является классификация фотографий собак и кошек, мы проверяем для каждого изображения, предсказывает ли модель CLIP текстовое описание «фотография собаки» или «фотография кошки» с большей вероятностью быть парным. с этим.

![3-1](https://user-images.githubusercontent.com/57997673/119855225-66d19c00-bf1a-11eb-89df-ff2d6d6e36f9.png)

CLIP предварительно обучает encoder для изображений и encoder для текста, чтобы предсказать, какие изображения были связаны с какими текстами в нашем наборе данных. Затем мы используем это поведение, чтобы превратить CLIP в zero-shot classifier. Мы конвертируем все классы набора данных в подписи, такие как «фотография собаки», и прогнозируем класс подписи. CLIP оценивает лучшие пары с данным изображением.

## Блок 4. Генерация клипа, CLIP + Taming Transformer
![image](https://user-images.githubusercontent.com/29739660/119980853-883b9200-bfc5-11eb-99c4-7c461d1bee36.png)

Разработанные для изучения взаимодействия на больших расстояниях с последовательными данными, трансформеры продолжают демонстрировать самые современные результаты в самых разных задачах. В отличие от CNN, они не содержат индуктивного смещения, которое отдает приоритет локальным взаимодействиям. Это делает их выразительными, но при этом невозможными с вычислительной точки зрения для длинных последовательностей, таких как изображения с высоким разрешением. Данная архитерктура демонстрирует, как сочетание эффективности индуктивного смещения CNN с выразительностью трансформеров позволяет им моделировать и, таким образом, синтезировать изображения с высоким разрешением. Архитектура демонстрирует, как использовать CNN для изучения контекстно-богатого словаря составляющих изображения, и, в свою очередь, использовать encoder для эффективного моделирования их композиции в изображениях с высоким разрешением. Данный подход легко применяется к задачам условного синтеза, где как непространственная информация, такая как классы объектов, так и пространственная информация, такая как сегментирование, может управлять сгенерированным изображением.

## Блок 5. Повышение разрешения, SuperResolution

Для решения задачи single image super resolution были использованы GAN-ы, а именно ESRGAN. ESRGAN (Enchanced-SRGAN) - это улучшенная версия нейросети SRGAN, поэтому в начале поговорим о ней.

SRGAN представляет собой генеративно-состязательную нейросеть для задачи получения фотореалистичных изображений высокого качества из изображений с плохим качеством.
Генеративно-состязательные сети - это алгоритм, построенный на комбинации из двух нейронных, одна из которых (генератор) генерирует образцы, а другая (дискриминатор) старается отличить правильные («подлинные») образцы от неправильных.
Архитектура генератора и дискриминатора, использованная в SRGAN:

<img width="863" alt="srgan_architecture" src="https://user-images.githubusercontent.com/57997673/120758283-4e651100-c51a-11eb-8b5d-4c2a37135b3f.png">

Успех SRGAN в задаче super resolution обусловлен следующими особенностями:
Генеративно состязательные сети позволяют создавать более реалистичные изображения, чем нейросети, основанные на оптимизации MSE между пикселями. Модели, ориентированные на оптимизацию MSE по пикселям, "усредняли" текстуры, что делало их чрезмерно гладкими. Использование GAN-ов, благодаря дискриминатору, который учится отличать фейковые сгенерированные изображения от настоящих, позволяет генерировать изображение из множества фотореалистичных вариантов, не усредняя все текстуры. 

![natural_manifold](https://user-images.githubusercontent.com/57997673/120758400-7785a180-c51a-11eb-82c2-1192deaf3608.png)

Второй важной особенностью стало использование Perceptual loss. В процессе обучения нейронной сети, оптимизацию по MSE между пикселями заменили на perceptual loss, который представляет из себя MSE, посчитанный в пространстве фичей глубокой сверточной сети (в частности, VGG19). Такая ошибка более инвариантна к изменению пикселей изображения, что предоставляет генератору большую свободу в изменении изображения.

### Что улучшили в ESRGAN
ESRGAN отличается от своего предшественника двумя основными нововведениями:

-Поменяли архитектуру генератора, заменив residual блоки на более крупные Residual-in-Residual Dense блоки (RRDB) без батч-нормализации.
-Обычный дискриминатор заменили на относительный (relativistic discriminator), который предсказывает вероятность того, что оригинальное изображение более реалистичное, чем сгенерированное.

Solution based on
* https://github.com/lucidrains/big-sleep
* https://github.com/snakers4/silero-models
* https://openai.com/blog/clip/
* https://openai.com/blog/jukebox/
* https://magenta.tensorflow.org/music-transformer
* https://arxiv.org/pdf/1809.00219.pdf
