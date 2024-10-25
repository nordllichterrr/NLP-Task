# NLP-TestTask
Этот репозиторий содержит решение тестового задания по извлечению сущностей из новостных текстов с помощью GigaChat. Решение реализовано в Jupyter Notebook и включает в себя этапы подготовки данных, получения ответов от GigaChat, анализа ошибок, оценки качества модели и визуализации результатов.


Задание 1. Описание задачи с точки зрения NLP.
Перед нами стоит задача извлечения сущностей из новостных документов (файлов). Задача NER – выделить спаны сущностей в тексте (спан – непрерывный фрагмент текста). Допустим, есть новостной текст, и мы хотим выделить в нем сущности (некоторый заранее зафиксированный набор — например, персоны, локации, организации, даты и так далее).  Сущности могут включать имена людей, организации, географические названия, даты и другие специфические термины. Например, в предложении "Борис Джонсон стал премьер-министром Великобритании" сущностями будут "Борис Джонсон" (PER) и "Великобритания" (LOC).

Классические методы решения задачи NER:
   1) Правила на основе шаблонов. Использование регулярных выражений и заранее заданных шаблонов для поиска сущностей. Этот метод требует глубоких познаний в грамматике языка и может быть ориентирован на ограниченное количество языков. Пример: создание шаблонов для распознавания дат, имен или названий.

   2) Словарные методы. Составление списков рассматриваемых слов в справочниках. Основным недостатком этого подхода является необходимость постоянного обновления и поддержки списков. Пример: использование базы данных имен людей или организаций.

  3) Статистические методы. Модели, основанные на условных случайных полях (CRF) или скрытых марковских моделях (HMM), которые используют вероятностные подходы для определения сущностей. Пример: использование CRF для классификации токенов текста на основе их контекста.

  4) Машинное обучение. Методы машинного обучения с учителем, такие как решающие деревья (Decision Trees), максимальная энтропия (Maximum Entropy), опорные векторы (Support Vector Machines, SVM) и другие. Эти методы требуют размеченных данных для обучения. Пример: обучение модели SVM на размеченных данных для классификации сущностей.

LLM решают задачу классификации, поскольку NER в этом случае сводится к классификации токенов. Для сведения к задаче классификации число классов обучаемой модели нейронной сети увеличивается в соответствии со схемой IOB. IOB (inside-outside-begin) — подход к разметке данных, при котором метку «B» (begin) получает токен-начало именованной сущности; «I» (inside) — токен внутри именованной сущности (не является началом); «O» (outside) — токен, не являющийся частью какой-либо именованной сущности. Одной из самых популярных групп LLM, практически State-of-the-Art в задачах NLP, являются архитектуры нейронных сетей, основанных на BERT. BERT (Bidirectional Encoder Representations from Transformers) в своей архитектуре использует механизм двунаправленного внимания. Он позволяет провести предварительную подготовку глубоких двунаправленных представлений немаркированного текста путем совместного обобщения левого и правого контекста во всех слоях.

Для оценки пользовательское распознавание именованных сущностей использует следующие метрики:
  1) Точность: измеряет, насколько точна модель. Представляет собой соотношение между правильно определенными положительными (истинноположительными) результатами и всеми определенными положительными значениями. Метрика точности показывает, сколько из спрогнозированных сущностей правильно помечено метками.
                                           Precision = #True_Positive / (#True_Positive + #False_Positive)

  2) Полнота: измеряет способность модели прогнозировать фактические положительные классы. Это отношение между спрогнозированными истинноположительными результатами и фактически помеченными значениями. Метрика полноты показывает, сколько из прогнозируемых сущностей определено верно.
                                           Recall = #True_Positive / (#True_Positive + #False_Negatives)

  3) F-мера: зависит от точности и полноты. Она необходима при поиске баланса между точностью и полнотой.
                                                F1 Score = 2 * Precision * Recall / (Precision + Recall).



Задание 2. Чтение датасета.
Реализуем чтение датасета в pandas DataFrame. Выведем шапку датафрейма:

![image](https://github.com/user-attachments/assets/e0cb3ddf-2d61-4d0e-8993-be3d877a3b2e)

Вот так выглядит сам датафрейм в формате .csv файла с выявленными сущностями, которые помещены в колонку "gold_answer":
![image](https://github.com/user-attachments/assets/22d3daee-9d9f-4645-99a2-526d90b8d459)


Задание 3. Функция, которая выдаёт текст входного сообщения для LLM.

Напишем функцию, которая принимает на вход строку датафрейма и выдает текст входного сообщения для LLM:
![image](https://github.com/user-attachments/assets/b2b752fa-bd28-4f15-996d-5fc449377e94)
![image](https://github.com/user-attachments/assets/de6fa69d-4622-4c1b-b2f2-f50f5db09ed9)

В датафрейме это выглядит следующим образом:
![image](https://github.com/user-attachments/assets/d6ba3e54-144a-40b2-9947-2adcad52ac85)

В .csv файле с текстами документов и сущностями появляется колока "llm_input", в которой написано входное сообщение для LLM.


Задание 4. Ответы Gigachat.
Прикреплю несколько скриншотов с историей сообщений с Gigachat, чтобы продемонстрировать подлинность ответов:
![image](https://github.com/user-attachments/assets/b955b7e7-8c26-4e32-a7dd-e84fb696b8ac)
![image](https://github.com/user-attachments/assets/e5d0b658-4937-4f39-a07e-3bf73493f4b7)
![image](https://github.com/user-attachments/assets/36d6c397-05b1-45c2-8840-5c285707d311)
![image](https://github.com/user-attachments/assets/129cae8e-751c-4ef1-9d06-60a925b04b36)
![image](https://github.com/user-attachments/assets/cf48c09d-5b30-4dce-8000-42e638481966)

Ответы, полученные от Gigachat я внесла в датафрейм:
![image](https://github.com/user-attachments/assets/34b7a054-c16c-4e5a-8e16-ef853aecd02e)
В датафрейме я выделила отдельный столбец для ответов от Gigachat - "gigachat_answers"


Задание 5. Реализация алгоритма подсчёта метрик и проведение юнит-тестов.
Напишем самостоятельно алгоритм для подсчета метрик score_fn(gold: str, pred: str) → float при помощи библиотек numpy, scipy, pandas:
![image](https://github.com/user-attachments/assets/a2599000-0927-4eec-9744-528cec7a5f5c)

Во второй части кода показано проведение юнит-тестов:
![image](https://github.com/user-attachments/assets/cbf2b54e-86dd-4d51-9fa2-d191daf7afe0)

Одним из приемов для ускорения работы функций подсчета метрик является векторизация вычислений, то есть использование функций, которые поддерживают операции над векторами. Это позволяет значительно повысить производительность, особенно при работе с большими объемами данных. Для работы с функцией score_fn мы могли бы вызывать ее в цикле для каждого элемента списка, но гораздо проще использовать векторизацию. По сути, векторизация преобразует функцию таким образом, что она начинает принимать весь вектор целиком, а не отдельный его элемент. Такой подход позволяет использовать преимущества библиотек, таких как NumPy, которые оптимизированы для работы с массивами данных. Вместо того чтобы итерировать по элементам и считать метрики по отдельности, мы использовали векторные операции для выполнения всех расчетов одновременно.

В задаче извлечения именованных сущностей (NER) важным аспектом является оценка качества предсказаний модели. Для этого используется функция score_fn, которая вычисляет F1-меру на основе истинных меток (gold) и предсказанных значений (pred). Выбор F1-меры в качестве основной метрики для оценки качества модели обусловлен несколькими факторами. Во-первых, F1-мера позволяет учитывать как количество правильно предсказанных сущностей, так и количество пропущенных, что особенно важно в задачах NER, где ложные срабатывания и пропуски могут иметь серьезные последствия. Во-вторых, в задачах извлечения сущностей часто наблюдается дисбаланс между количеством положительных и отрицательных примеров. F1-мера помогает избежать ситуации, когда высокая точность достигается за счет игнорирования значительного числа истинных положительных примеров. Наконец, F1-мера является универсальной метрикой, которая может быть использована в различных контекстах, что делает ее особенно полезной для оценки моделей в NLP.


Задание 6. Вычисление метрик в датафрейме.

Напишем функцию для подсчёта метрик:
![image](https://github.com/user-attachments/assets/8b7238ac-d150-411b-8e16-5e762ca9343d)

Создадим список для хранения метрик и датафрейм с метриками:
![image](https://github.com/user-attachments/assets/a0c3b090-3609-41c0-9c60-9d9682a6b065)

Агрегация результатов подсчёта метрик по каждой сущности:
![image](https://github.com/user-attachments/assets/96ceaa6d-470f-4393-b433-f54feb1f5ad7)

Агрегация результатов по каждому документу:
![image](https://github.com/user-attachments/assets/1dedcb27-200b-44ab-aec6-e8a265a9e1e9)

Покажем полученные результаты на графиках:

![image](https://github.com/user-attachments/assets/efb1933e-465c-4175-b894-a1da80ea71de)
![image](https://github.com/user-attachments/assets/e6201364-81a9-44bb-a9fb-5db322d22172)

На обоих графиках - по сущностям и документам - мы видим, что все столбцы по f1-мере достигают значения 1.0 . Это говорит нам о том, что модель правильно идентифицировала все сущности без ложных срабатываний и пропусков. Это указывает нам на идеальное предсказание модели, что означает, что она правильно идентифицировала все сущности без ложных срабатываний и пропусков. Такая оценка указывает на то, что классификатор точен и надёжен, не пропускает значительное количество экземпляров. 


Задание 7. Выявление зависимости метрик от длины документа.

При помощи библиотеки seaborn создадим pairplot - мощный инструмент визуализации, используемый для анализа зависимостей между несколькими переменными в одном наборе данных. Он создает матрицу диаграмм рассеяния, показывающую попарные взаимосвязи между переменными в наборе данных, помогая визуализировать корреляции и распределения:
![image](https://github.com/user-attachments/assets/396c730d-c2f4-4dc8-a598-0aabcfe0061a)

















