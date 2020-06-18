# Event2mind for Russian: dataset and model


## Dataset

См папку dataset:

- dataset_clean.csv данные собранные в Толоке
- new_rocstory.csv переведенная и отредактированная часть датасета

Смердженные и пошафленные данные:

- train_v2.csv
- dev_v2.csv

Первая версия датасета:

- rus_dev.csv
- rus_train.csv

## Как Обучить и тестить Event2Mind


Версия allennlp v.0.8.2 (все восьмые тоже идут, последние уже не поддерживают)


###  Prediction

```
from allennlp.predictors.predictor import Predictor

predictor = Predictor.from_path(
    "https://s3-us-west-2.amazonaws.com/allennlp/models/event2mind-2018.10.26.tar.gz"
)
predictor.predict(source="PersonX пьет кофе")
```

### Training

В командной строке:

```
allennlp dry-run -o '{"dataset_reader": {"dummy_instances_for_vocab_generation": true}} {"vocabulary": {"min_count": {"source_tokens": 2}}}' training_config/event2mind.json --serialization-dir vocab_output_path
allennlp train -o '{"vocabulary": {"directory_path": "vocab_output_path/vocabulary/"}}' training_config/event2mind.json --serialization-dir output_path
```

### Evaluation

```
allennlp evaluate \
    https://s3-us-west-2.amazonaws.com/allennlp/models/event2mind-2018.10.26.tar.gz  \
    https://raw.githubusercontent.com/uwnlp/event2mind/9855e83c53083b62395cc7e1af6ee9411515a14e/docs/data/test.csv
```

Пример конфига можно посмотреть в архиве модели в папке models.

Вектора использовались с сайта RusVectores, из коробки советую использовать fasttext, word2vec если с тегами частей речи не заведется, если не править исходники.


## Publications

- Dialogue 2020 https://www.youtube.com/watch?v=nnHgex4_00I
- Оригинал статьи английской https://arxiv.org/pdf/1805.06939.pdf
