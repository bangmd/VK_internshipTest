## Test - Стартовый проект для тестового задания в команду Рейтингов и Отзывов ВК.

Скринкаст итоговой работы|Общее количество отзывов|Экран просмотра фото из отзыва
-|-|-
![Скринкаст итоговой работы](/Screenshots/vid_1.gif) | ![Общее количество отзывов](/Screenshots/all.png) | ![Экран просмотра фото из отзыва](/Screenshots/full.png)

🛠 Используемый стек технологий:
- UIKit
- URLSession 
- NSCache 
- GCD
- UICollectionView 
- UITableView 


## Что сделано:
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
   - Реализован метод didTapShowMore(), который вызывает замыкание onTapShowMore с уникальным идентификатором ячейки, что снимает ограничение на количество строк и разворачивает текст отзыва.

6. **Асинхронная загрузка изображений:**
  - В JSON файл отзыва добавлено новое поле avatar_url содержащие валидные ссылки на изображения.
  - Реализована асинхронная загрузка изображений с помощью класса ReviewsImageLoader, который использует URLSession для загрузки и NSCache для кэширования полученных изображений.
  - В ячейках отзывов, если поле avatar_url имеет ссылку, то загружается изображение аватара асинхронно и устанавливается в avatarImageView, а иначе устанавливается placeholder из assets.
7. Кастомный индикатор загрузки для кнопки "Показать полностью..."

- Вместо мгновенного раскрытия текста добавлена плавная индикация процесса.
- При нажатии на кнопку "Показать полностью..." теперь отображается кастомный индикатор загрузки (анимированное кольцо).
- Индикатор анимируется в течение 0.5 секунд, после чего скрывается и текст раскрывается.
- Реализовано с помощью CALayer и CABasicAnimation.

## Дополнительные задачи

### Pull-to-Refresh
- Добавлен UIRefreshControl для списка отзывов:
  - При использовании механизма, срабатывает событие обновления.
  - Старые отзывы остаются на экране, пока загружаются новые.
  - После успешной загрузки, все элементы (начиная с offset = 0) заменяются на новые отзывы.
  - Затем таблица обновляется, и Pull-to-Refresh индикатор закрывается.

### Состояние загрузки отзывов

- Реализован индикатор загрузки при первом запуске приложения:
  - Во время первой загрузки отзывов (offset == 0) скрывается таблица и отображается индикатор загрузки. После завершения загрузки индикатор скрывается, и таблица становится видимой.
  - При повторных обновлениях (Pull-to-Refresh) индикатор загрузки не отображается – используется только UIRefreshControl.
  - Используется логика разделения состояний (isInitialLoading, isRefreshing), чтобы избежать одновременного отображения обоих индикаторов.

### Отображение фото в отзыве

- Теперь каждый отзыв может содержать от 0 до 5 фотографий (фото берутся из Assets проекта).
- Используется UICollectionView для отображения изображений в виде горизонтальной галереи. При отсутствии фото UICollectionView скрывается.
- Размер ячейки отзывов динамически изменяется в зависимости от количества фото.

### Экран просмотра фото (PhotoViewerViewController)
- Добавлен новый экран для просмотра фото из отзывов.
- Реализована кнопка закрытия в правом верхнем углу:
  - Позволяет вернуться назад к ленте отзывов.

#### Улучшение верстки:
- Фото располагаются между рейтингом и текстом отзыва.
- Если в отзыве есть фото, текст сдвигается вниз.
- Все отступы и размеры вынесены в ReviewCellLayout, обеспечивая адаптивность интерфейса.


## Выполненное задание:

Минимальный вариант|Максимальный вариант|Ячейка количества отзывов
-|-|-
![Минимальный вариант](/Screenshots/1.png) | ![Максимальный вариант](/Screenshots/max.png) | ![Ячейка количества отзывов](/Screenshots/all.png)
