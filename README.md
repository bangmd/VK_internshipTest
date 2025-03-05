# Test
Стартовый проект для тестового задания в команду Рейтингов и Отзывов ВК.

## Что сделано

1. **Ячейка отзыва**
   - Добавлен аватар пользователя, имя, рейтинг, текст отзыва и дата.
   - Кнопка «Показать полностью...» увеличивает число строк и обновляет ячейку.

2. **Ячейка количества отзывов**
   - В конце списка одна ячейка, показывающая «Всего N отзывов».
   - Старую ячейку удаляем перед добавлением новой, чтобы не было дубликатов.

3. **Предотвращение утечек памяти**
   - В замыканиях используем [weak self], чтобы избежать retain cycle.

4. **Улучшение производительности:**
   - Кэшируем высоты ячеек в heightCache, чтобы не пересчитывать boundingRect при каждом скролле.
   - Метод getReviews() выполняется на глобальной очереди, чтобы не блокировать главный поток. После получения данных обновление UI происходит на главном потоке, что обеспечивает плавность интерфейса.
   - При нажатии «Показать полностью» кэш для конкретной ячейки сбрасывается.

5. **Работа кнопки "Показать полностью"**
   - Добавлен target для кнопки «Показать полностью» в ячейке отзыва, чтобы при нажатии отзыв отображался полностью.
   - Реализован метод `didTapShowMore()`, который вызывает замыкание `onTapShowMore` с уникальным идентификатором ячейки, что снимает ограничение на количество строк и разворачивает текст отзыва.

6. **Асинхронная загрузка изображений:**
  - В JSON файл отзыва добавлено новое поле avatar_url содержащие валидные ссылки на изображения.
  - Реализована асинхронная загрузка изображений с помощью класса ReviewsImageLoader, который использует URLSession для загрузки и NSCache для кэширования полученных изображений.
  - В ячейках отзывов, если поле avatar_url имеет ссылку, то загружается изображение аватара асинхронно и устанавливается в avatarImageView, а иначе устанавливается placeholder из assets.



## Примеры выполненного задания:

Минимальный вариант|Максимальный вариант|Ячейка количества отзывов
-|-|-
![Минимальный вариант](/Screenshots/1.png) | ![Максимальный вариант](/Screenshots/2.png) | ![Ячейка количества отзывов](/Screenshots/3.png)
