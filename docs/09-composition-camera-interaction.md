# 09 — Большой справочник композиции, камеры, ракурсов и взаимодействия персонажей

Практическое руководство для Stable Diffusion WebUI Forge и `waiIllustriousSDXL_v170.safetensors`. Оно объединяет рабочие приёмы из этой базы знаний и документации Forge Couple, ControlNet и OpenPose: как поставить персонажа в кадре, задать перспективу, расстояние, ракурс, рост, контакт тел и рук, а также как управлять двумя разными персонажами без смешивания внешности.

> **Главное правило:** промпт задаёт пожелания, а не жёсткие координаты. Если важно точное положение тела, лица, кистей или камеры, комбинируй текст с Forge Couple, референсом, OpenPose/ControlNet и локальным Inpaint. Проверяй изменения на одном и том же seed.

## Содержание

1. Быстрый алгоритм композиции
2. Словарь кадрирования и масштаба
3. Угол камеры и направление взгляда
4. Вид спереди, сбоку, сзади и три четверти
5. POV / вид от первого лица
6. Расстояние до персонажа и глубина кадра
7. Взаимодействие: стоять рядом, касаться, обнимать, прижиматься
8. Высота, рост и точное назначение признаков персонажу
9. Forge Couple v7.1.0: Basic, Advanced, Mask и Global Effect
10. Готовые промпты-сценарии
11. ControlNet, OpenPose, референсы и Inpaint
12. Настройки генерации и рабочий порядок
13. Диагностика ошибок
14. Словарь английских тегов
15. План тестирования
16. Источники и ограничения

---

## 1. Быстрый алгоритм композиции

Не начинай с огромного списка тегов. Сначала ответь на шесть вопросов и расположи ответы в промпте в таком порядке:

1. **Кто в кадре?** `1girl`, `2girls`, `1boy`, `2people`, `solo`.
2. **Что видно?** `full body`, `upper body`, `cowboy shot`, `close-up`.
3. **Где камера?** `first-person view`, `from side`, `from behind`, `low angle`, `high angle`.
4. **Где персонажи?** `on the left`, `on the right`, `in the foreground`, `in the background`.
5. **Что делают друг с другом?** `holding hands`, `hugging`, `leaning against`, `looking at each other`.
6. **Какие детали отличают каждого?** Волосы, глаза, одежда, аксессуары, рост и поза.

Затем добавь место действия, свет и стиль. Для WAI Illustrious полезно начать с коротких, распространённых тегов, а не пытаться описать каждый пиксель прозой.

### Формула промпта

```text
masterpiece, best quality, amazing quality,
[число персонажей], [тип кадра], [ракурс камеры],
[расположение персонажей], [действие/контакт],
[фон], [свет], [стиль]

[регион/персонаж слева или первый]
[регион/персонаж справа или второй]
```

В Forge Couple строки с регионами должны быть разделены настоящими переносами строки, если в `Couple Separator` задан `\\n`. Не путай визуальный перенос длинного текста в поле с настоящим Enter. В режиме `Basic` каждая строка становится отдельной областью. Если общая строка сцены должна влиять на весь кадр, выбери `Global Effect = First Line`: первая строка будет общей, следующие две — регионами персонажей. Если `Global Effect = None`, не оставляй отдельную общую строку перед регионами — иначе она станет ещё одной областью.

---

## 2. Кадрирование: сколько персонажа видно

| Тег | Что обычно означает | Когда использовать |
|---|---|---|
| `extreme close-up` | Очень крупная деталь: глаз, рот, рука | Деталь или эмоция; не для описания всей позы |
| `close-up` | Лицо или голова с небольшой частью плеч | Эмоция, выражение лица |
| `portrait` | Портрет, часто голова и плечи | Акцент на лице и причёске |
| `upper body` | Верхняя часть тела | Объятие, руки на груди/плечах |
| `bust` | Голова и грудная часть | Иллюстративный портрет |
| `cowboy shot` | Примерно от середины бедра вверх | Жесты, одежда и выражение лица |
| `thighs` | Бёдра заметны в кадре | Одежда и поза ног частично |
| `knees up` | От колен и выше | Полуфигура, взаимодействие |
| `full body` | Персонаж целиком, включая ноги | Рост, обувь, стойка |
| `wide shot` | Широкий план, персонаж меньше относительно окружения | Место действия и расстояние |
| `long shot` | Дальний/общий план | Персонаж в пространстве |
| `extreme long shot` | Очень широкий план, окружение доминирует | Силуэт вдали, масштаб среды |

Теги не задают точный обрез по пикселям. Если написать `full body` и `close-up` одновременно, они могут конкурировать. Выбери один основной тип кадра, а вторичный добавляй только если понимаешь, зачем.

### Положение персонажа в кадре

- `centered composition` — персонаж по центру.
- `off-center` — смещён от центра.
- `on the left` / `on the right` — слева / справа.
- `in the foreground` — на переднем плане.
- `in the background` — на заднем плане.
- `between two people` — между двумя персонажами.
- `looking at viewer` — смотрит на зрителя/камеру.
- `looking at another` / `looking at each other` — смотрит на другого / друг на друга.
- `negative space` — свободное пространство вокруг главного объекта.
- `rule of thirds` — композиция с размещением важных объектов около третей кадра.

**Важно:** `left` и `right` описывают стороны изображения, а не обязательно левую/правую руку персонажа. Для сложной сцены добавляй `from viewer's perspective`, если это подходит, но проверяй результат.

---

## 3. Угол камеры и направление взгляда

Угол камеры и направление тела — разные вещи. Например, `from side` говорит о виде камеры, а `facing viewer` — куда направлен персонаж. Для более стабильного результата задавай их отдельно.

| Тег | Эффект | Пример применения |
|---|---|---|
| `eye level` | Камера примерно на уровне глаз | Естественный нейтральный кадр |
| `low angle` | Камера ниже, смотрит вверх | Персонаж кажется выше/внушительнее |
| `high angle` | Камера выше, смотрит вниз | Вид сверху на персонажа |
| `from above` | Сверху | Уточняй, насколько сильно сверху |
| `from below` | Снизу | Подчёркивает рост и силуэт |
| `bird's-eye view` | Почти вертикально сверху | Геометрия пола/пространства |
| `worm's-eye view` | Очень низкая точка съёмки | Сильная перспектива снизу |
| `dutch angle` / `tilted angle` | Наклонённый горизонт | Динамика или тревожность |
| `overhead shot` | Камера прямо над сценой | Персонаж/предметы под камерой |
| `over-the-shoulder` | Камера из-за плеча одного персонажа | Разговор, взаимодействие |
| `POV` / `first-person view` | Взгляд от первого лица | Зритель занимает место участника сцены |
| `from the side` | Вид сбоку | Профиль и контакт двух фигур |
| `three-quarter view` | Три четверти, не полный профиль | Объём лица и тела |
| `from behind` | Вид сзади | Спина, затылок, взгляд вперёд |
| `profile` / `profile view` | Профиль | Чёткий боковой силуэт |
| `front view` | Спереди | Лицо и корпус направлены к камере |
| `rear view` | Сзади | Спина обращена к камере |

### Как добиться «девушка сверху»

Сначала уточни, что именно значит «сверху»:

- **Камера смотрит на неё сверху:** `high angle, looking down at the girl`.
- **Камера прямо над ней:** `overhead shot, bird's-eye view`.
- **Девушка стоит выше камеры:** `low angle, looking up at the girl, towering perspective`.
- **Она находится на возвышении:** `standing on stairs`, `standing on a platform`, `on a higher step`.
- **Она наклоняется к камере сверху:** `leaning toward the viewer, viewed from below`.

Не смешивай эти смыслы в одной строке. `high angle` и `low angle` — противоположные указания для камеры, поэтому одновременно обычно мешают друг другу.

### Перспектива и искажение

- `wide-angle perspective` — широкоугольная перспектива, объекты ближе к камере могут казаться крупнее.
- `foreshortening` — перспективное укорочение конечностей/частей тела, направленных к камере.
- `dramatic perspective` — выразительная перспектива.
- `depth of field` — глубина резкости; описывай, что именно резкое, а что размыто.
- `shallow depth of field` — малая глубина резкости, фон часто размыт.
- `deep depth of field` — большая глубина резкости, больше сцены в фокусе.

Эти термины не заменяют контроль позы. Если кисть должна быть направлена прямо в объектив, текст может не удержать точную геометрию — понадобится референс или локальная правка.

---

## 4. Вид спереди, сбоку, сзади и три четверти

### Спереди

```text
front view, facing viewer, eye level, upper body,
shoulders visible, looking at viewer
```

### Профиль

```text
side view, profile, facing left, eye level,
full body, clear side silhouette
```

Чтобы персонаж смотрел влево, а камера видела его профиль, используй оба понятия: `profile view, facing left`. Один тег `looking left` может повернуть только глаза или голову, а не весь корпус.

### Три четверти

```text
three-quarter view, body turned slightly,
face visible, looking toward viewer
```

Это полезный универсальный ракурс: лицо остаётся читаемым, но корпус выглядит объёмнее, чем в строго фронтальном виде.

### Вид сзади

```text
from behind, back view, looking toward the scenery,
full body, hair seen from behind
```

Если нужно видеть лицо через плечо, добавь `looking back over shoulder`. Если написать только `from behind`, лицо может вообще не появиться.

### Камера из-за плеча

```text
over-the-shoulder shot, viewer behind the first character,
second character facing the camera, conversation
```

Полезно для диалога двух персонажей. В Forge Couple важно отдельно назначить промпт каждой области; одна лишь фраза `over-the-shoulder` не гарантирует правильную композицию.

---

## 5. POV — вид от первого лица

`POV` и `first-person view` подсказывают модели, что камера находится на месте участника сцены. Но они не гарантируют, что в кадре появятся руки/торс наблюдателя или что второй персонаж окажется в правильном масштабе. Укажи видимые признаки явно.

### 5.1. Девушка стоит прямо перед зрителем

```text
first-person view, POV, eye-level perspective,
a girl standing directly in front of the viewer,
looking at viewer, close distance, upper body,
her face clearly visible, natural perspective,
indoor room, soft lighting
```

### 5.2. Девушка рядом с телом зрителя и прижимается к нему

Если подразумевается близкий контакт, не пиши, что она далеко. Используй слова о близком расстоянии и точках контакта:

```text
first-person view, POV, a girl standing very close to the viewer,
leaning against the viewer's torso, her shoulder resting against the viewer,
one hand holding the viewer's sleeve, looking up at the viewer,
close distance, intimate framing, upper body,
natural body contact, relaxed posture
```

Если нужно показать руку/одежду зрителя на переднем плане, добавь `viewer's arm in foreground`, `viewer's shoulder in foreground` или `viewer’s hands visible` — но только то, что действительно должно попасть в кадр. Не добавляй `full body`, если камера буквально стоит вплотную: это часто конфликтующие задачи.

### 5.3. Девушка стоит рядом, но не касается

```text
first-person view, POV, a girl standing beside the viewer,
close to the viewer, shoulder-to-shoulder,
her body turned slightly toward the viewer, looking at viewer,
medium shot, natural perspective
```

### 5.4. Девушка далеко в кадре

```text
first-person view, POV, a girl standing far away across the room,
small figure in the distance, wide shot, full environment visible,
large negative space, long perspective
```

### Важное различие: «далеко» и «прижимается к телу»

Это разные пространственные требования. Если девушка прижимается к телу наблюдателя, она находится рядом с камерой. Если она далеко, контакт с телом невозможен в той же физической сцене. Возможно, ты имеешь в виду:

- она **далеко от камеры, но близко к другому персонажу**;
- она **рядом с камерой и прижимается к зрителю**;
- она **находится на заднем плане, но прижимается к телу персонажа, который тоже там**.

Назови, кто именно к кому прижимается и где стоит камера. Не пытайся исправить противоречие десятком тегов.

### 5.5. Вид от первого лица с объятием

```text
POV, first-person view, a girl hugging the viewer,
her arms around the viewer, viewer's arms partially visible,
close-up, face near the viewer, looking at viewer,
soft warm light, natural pose
```

Для контроля кистей лучше сначала получить правильную позу, затем отдельно исправить руки через Inpaint или использовать референс позы.

---

## 6. Расстояние до персонажа и глубина кадра

В промпте полезно разделять три независимых параметра:

1. **Дистанция камеры до персонажа:** близко / средне / далеко.
2. **Размер персонажа в кадре:** крупный / средний / маленький.
3. **Положение относительно другого персонажа:** рядом / впереди / позади / выше / ниже.

### Словарь расстояний

| Тег | Смысл |
|---|---|
| `close-up` | Крупное кадрирование |
| `close to viewer` | Персонаж близко к зрителю |
| `standing close together` | Персонажи стоят близко друг к другу |
| `standing side by side` | Стоят рядом |
| `a few steps away` | В нескольких шагах |
| `in the distance` | Вдали |
| `far away` | Далеко |
| `small figure in the distance` | Маленькая фигура вдали |
| `foreground` | Передний план |
| `midground` | Средний план |
| `background` | Задний план |
| `depth` / `depth perspective` | Ощущение глубины пространства |

Не полагайся только на `close` или `far`: добавляй кадрирование и масштаб. `full body, close to viewer` означает, что персонаж целиком занимает значительную часть кадра; `wide shot, far away, small figure` — что он занимает небольшую часть изображения.

### Как показать, что один персонаж впереди другого

```text
two girls, one in the foreground and one in the background,
clear depth separation, both visible, different sizes in perspective
```

Если передний персонаж должен быть выше/крупнее из-за перспективы, не путай это с реальным ростом. Напиши отдельно `taller girl` для физического роста и `closer to camera` для расстояния до камеры.

---

## 7. Взаимодействие: контакт и действия

Самое сложное в сценах с двумя людьми — не само число персонажей, а контакт: руки должны принадлежать нужным персонажам, точки касания должны совпадать, а тела не должны сливаться.

### 7.1. Стоят рядом

```text
2girls, standing side by side, full body,
both visible, looking at each other, natural posture
```

### 7.2. Смотрят друг на друга

```text
2girls, standing face to face, looking at each other,
eye contact, relaxed posture, both faces visible
```

### 7.3. Держатся за руки

```text
2girls, facing each other, holding hands,
hands meeting between them, looking at each other,
full body, natural arm positions
```

### 7.4. Объятие

```text
2girls, hugging, arms around each other,
bodies close together, natural contact, both faces visible
```

Уточняй тип объятия, если важна композиция: `side hug`, `front hug`, `one arm around shoulders`, `arms around waist`. Не добавляй сразу несколько несовместимых вариантов.

### 7.5. Одна опирается на другую

```text
2girls, one girl leaning against the other girl,
shoulder resting against her shoulder, relaxed posture,
standing close together, natural body contact
```

### 7.6. Девушка прижимается к корпусу персонажа

```text
a girl leaning against another character's torso,
her head resting on the character's chest,
one hand holding the character's shirt,
close contact, relaxed posture, both characters visible
```

Укажи владельца точки контакта. Формулировка `her head resting on the other girl's shoulder` яснее, чем просто `leaning`.

### 7.7. Разговор на разной высоте

```text
2girls, one taller girl and one shorter girl,
standing face to face, looking at each other,
the shorter girl looking upward, the taller girl looking down,
full body, natural posture
```

### 7.8. Персонажи должны не перекрывать друг друга

Добавляй `both characters fully visible`, `clear separation between characters`, `distinct silhouettes`, `no overlapping faces`. Если они должны соприкасаться, не требуй `no overlap` для всего тела — уточни, что лица и силуэты различимы, а руки/плечи соприкасаются.

---

## 8. Высота, рост и назначение признаков персонажу

Чтобы конкретная девушка была высокой, мало написать `tall` в общем промпте. При нескольких персонажах связывай рост с её устойчивыми признаками: волосами, одеждой, местом и позой.

### 8.1. Высокая блондинка слева, низкая черноволосая справа

```text
masterpiece, best quality, amazing quality,
2girls, full body, standing side by side, looking at each other,
school courtyard, eye level

2girls, tall girl, blonde hair, blue eyes, long blonde hair,
white blouse, blue pleated skirt, long legs,
standing on the left, noticeably taller than the other girl,
looking down slightly at the other girl

2girls, short girl, black hair, red eyes, long black hair,
black jacket, red pleated skirt, petite figure, shorter legs,
standing on the right, noticeably shorter than the blonde girl,
looking up at the blonde girl
```

Это пример структуры. В реальном Forge Couple проверь, как конкретно твоя версия обрабатывает общую строку и строки регионов. Не добавляй противоречивые указания вроде `tall` и `short` в одну региональную строку.

### 8.2. Слова роста

- `tall girl` — высокая девушка.
- `short girl` — невысокая девушка.
- `petite` — миниатюрное телосложение, не строгая мера роста.
- `long legs` — длинные ноги.
- `shorter than the other girl` — ниже другой девушки.
- `much taller than the other girl` — заметно выше другой девушки.
- `looking up at` — смотрит вверх на кого-то.
- `looking down at` — смотрит вниз на кого-то.

Сравнительные фразы могут работать слабее частых коротких тегов. Сочетай их с композицией: ступеньки, разный уровень пола, наклон головы, видимая разница в длине ног.

### 8.3. Как закрепить «именно эта девушка»

Используй **один и тот же набор признаков в одной строке региона**:

- персонаж: `blonde hair, blue eyes`;
- одежда: `white blouse, blue skirt`;
- позиция: `on the left`;
- рост: `tall girl`;
- действие: `looking down at the other girl`.

Для второго персонажа создай отдельную строку с другим набором. Избегай общих описаний вроде `tall, blonde, black hair, short` без ясной группировки: модель может смешать атрибуты.

**Идентичность не закрепляется навсегда одним словом.** Для повторяемого персонажа между разными изображениями лучше использовать референс/character LoRA (если есть), а для одного кадра — Forge Couple и аккуратное региональное описание.

---

## 9. Forge Couple v7.1.0: как использовать режимы

Официальная документация расширения: https://github.com/Haoming02/sd-forge-couple

Forge Couple применяет разные текстовые условия к областям изображения. Это снижает смешивание признаков, но **не превращает области в жёсткие маски для каждого пикселя**. Результат всё ещё зависит от чекпойнта и понятности сцены.

### 9.1. Базовые настройки для двух персонажей

Для первого теста:

- **Enable:** включить.
- **Region Assignment:** `Basic`.
- **Couple Separator:** `\\n` — разделение по настоящему переносу строки.
- **Tile Direction:** `Horizontal` — строки распределяются слева направо.
- **Global Effect:** сначала `None`, если хочешь проверить только региональное разделение.
- **Compatibility:** включай только если нужно обходить несовместимость/проблемы на Hires.fix; проверь подсказку своей версии.
- **Hires.fix:** на первом тесте выключен.

После того как простая схема работает, можно проверить Global Effect. В Basic и Mask доступно применение первой или последней строки как общего фона/эффекта. Если включён Global Effect, не забывай о дополнительной строке: официальная инструкция указывает, что Basic нуждается минимум в двух строках для регионов и в трёх, когда включён Global Effect. Поэтому при `First Line` для двух персонажей нужны: одна общая строка и две строки регионов.

### 9.2. Basic — две области слева направо

В Basic каждая строка позитивного промпта соответствует области изображения. При `Horizontal` первая область слева, следующая справа; при `Vertical` строки распределяются сверху вниз.

Простой тест **без Global Effect** — две строки:

```text
2girls, blonde hair, blue eyes, white blouse, blue skirt, tall girl, standing on the left, looking at the other girl
2girls, black hair, red eyes, black jacket, red skirt, short girl, standing on the right, looking at the blonde girl
```

Сначала проверь, что левая и правая области соответствуют ожиданиям. Затем добавляй действие и фон. В каждой строке региона указывай общее число персонажей (`2girls`) согласно рекомендациям автора расширения.

Если нужен общий фон/качество, а твоя версия использует Global Effect через первую/последнюю строку, настрой это отдельно и проверь счёт строк. Не вставляй пустые строки: они тоже могут учитываться как отдельные строки промпта.

### 9.3. Global Effect — общий фон

Общий эффект подходит для описания того, что должно действовать на всё изображение: фон, освещение, качество или общий стиль. Начни с веса по умолчанию или около `0.5`, а затем сравнивай. Слишком сильный общий текст может конкурировать с локальными признаками персонажей.

Пример общей строки:

```text
masterpiece, best quality, anime illustration, school courtyard, soft daylight, 2girls, full body, standing face to face, holding hands
```

Не помещай в общий блок конкретную одежду одной девушки, если не хочешь, чтобы она влияла на второго персонажа.

### 9.4. Advanced — области с координатами

Advanced нужен, если Basic делит изображение слишком грубо, например:

- персонаж слева, но не на всю высоту кадра;
- персонаж на переднем плане в центре;
- одна фигура занимает верхнюю часть, другая — нижнюю;
- POV-сцена, где один персонаж рядом с камерой, а другой занимает боковую область.

Координаты описывают долю изображения от `0.0` до `1.0`:

- `x`: слева направо;
- `y`: сверху вниз;
- `0.0–1.0` по обеим осям — весь кадр.

Обычно области задаются прямоугольниками и весом. Важно: **всё изображение должно быть покрыто весами**, иначе расширение может выдать ошибку. Если у тебя нет уверенности, начни с Default Mapping, затем перетаскивай прямоугольники мышью. Подсказки UI могут отличаться между версиями.

Пример логики областей для горизонтальной сцены:

- общий фон: покрытие всего кадра, если конфигурация требует отдельного слоя;
- девушка слева: примерно `x 0.0–0.58, y 0.0–1.0`;
- девушка справа: примерно `x 0.42–1.0, y 0.0–1.0`.

Небольшое перекрытие областей может помочь контакту рук/плеч. Это стартовая геометрия, не обязательные универсальные числа. Проверь, что фон и персонажи имеют покрытие всей площади.

### 9.5. Mask — области вручную

Mask полезен, когда прямоугольники Advanced слишком грубые. Можно нарисовать область, соответствующую приблизительному силуэту персонажа или зоне действия. По инструкции расширения, в маске учитываются чисто белые пиксели; другие цвета не считаются областью. Регионы не пиксельно-точные — рисуй общую зону, а не каждый контур волоса.

Используй Mask, когда:
- две фигуры стоят не по краям, а диагонально;
- одна фигура перекрывает часть другой;
- нужно выделить конкретную область переднего плана;
- Basic регулярно смешивает одежду.

### 9.6. Couple Separator

Обычно разделитель — перенос строки `\\n`. Поэтому:
- нажимай Enter между строками;
- не вставляй текст `\\n` в поле промпта как обычные символы, если только не настраиваешь separator именно так;
- не оставляй пустую строку в конце;
- следи, чтобы число строк совпадало с числом областей;
- сторонние расширения обработки промптов могут менять переносы.

### 9.7. Что Forge Couple не делает

- Не гарантирует конкретную анатомию кистей.
- Не фиксирует точный угол камеры.
- Не гарантирует сохранение одной личности между разными генерациями.
- Не заменяет OpenPose, референс или Inpaint, если поза критична.
- Не исправляет противоречия в промпте автоматически.

---

## 10. Готовые промпты-сценарии

Все примеры ниже — заготовки. Если используешь Basic, настрой число строк в соответствии с выбранным Global Effect. Сначала тестируй без дополнительных LoRA и сложного ControlNet.

**Правило для примеров с тремя строками (общая сцена + два персонажа):** в `Region Assignment: Basic` поставь `Global Effect = First Line`, чтобы первая строка была общей, а две следующие — регионами персонажей. Если хочешь тестировать с `Global Effect = None`, убери общую первую строку и перенеси фон/свет в региональные строки; иначе первая строка станет ещё одной областью. Для одиночных POV-примеров с одним персонажем Forge Couple обычно не нужен — отключи его, если не задаёшь несколько областей.

### A. Две девушки стоят рядом, видны целиком

```text
masterpiece, best quality, amazing quality, anime illustration,
2girls, full body, standing side by side, both fully visible,
school courtyard, eye level, soft daylight

2girls, blonde hair, blue eyes, white blouse, blue pleated skirt,
black knee-high socks, brown loafers, standing on the left,
looking at the other girl

2girls, long black hair, red eyes, black jacket, red pleated skirt,
black boots, standing on the right, looking at the blonde girl
```

### B. Высокая блондинка и низкая черноволосая, лицом друг к другу

```text
masterpiece, best quality, 2girls, full body,
standing face to face, looking at each other, clear height difference,
school courtyard, eye level, natural posture

2girls, tall girl, blonde hair, blue eyes, long blonde hair,
white blouse, blue skirt, long legs, standing on the left,
noticeably taller, looking down slightly

2girls, short girl, black hair, red eyes, black jacket,
red skirt, petite figure, standing on the right,
noticeably shorter, looking up at the blonde girl
```

### C. POV: девушка стоит близко и прижимается к зрителю

```text
masterpiece, best quality, anime illustration,
POV, first-person view, eye-level perspective,
a girl standing very close to the viewer,
leaning against the viewer's torso, her head near the viewer's shoulder,
one hand holding the viewer's sleeve, looking up at the viewer,
close distance, upper body, natural body contact,
soft indoor lighting, cozy room

1girl, long brown hair, gentle expression, white sweater,
standing close to the viewer, leaning against the viewer,
her hand gripping the viewer's sleeve
```

Если хочешь показать руки наблюдателя, добавь `viewer's hand in foreground` или `viewer's arm around her shoulders` только когда это соответствует задумке. Для POV с одним видимым персонажем Forge Couple может не понадобиться.

### D. POV: девушка далеко в комнате

```text
masterpiece, best quality, anime illustration,
POV, first-person view, wide shot, eye level,
a girl standing far away across the room,
small figure in the distance, full environment visible,
large negative space, strong depth perspective,
warm light from the window

1girl, long brown hair, white sweater, standing near the window,
looking toward the viewer
```

### E. Вид сбоку: одна девушка приобнимает другую за плечи

```text
masterpiece, best quality, anime illustration,
2girls, full body, side view, profile view,
standing close together, one girl putting an arm around the other's shoulders,
both faces partly visible, natural body contact,
city street, evening light

2girls, blonde hair, blue eyes, white jacket, standing on the left,
arm around the other girl's shoulders, looking at her

2girls, black hair, red eyes, dark jacket, standing on the right,
leaning slightly toward the blonde girl, smiling
```

### F. Камера снизу, высокая девушка кажется выше

```text
masterpiece, best quality, anime illustration,
low angle, looking up at a tall girl, full body,
dramatic perspective, long legs, standing confidently,
sky visible behind her

1girl, tall girl, long blonde hair, blue eyes,
white blouse, blue skirt, looking down toward the viewer
```

### G. Камера сверху, девушка смотрит вверх

```text
masterpiece, best quality, anime illustration,
high angle, looking down at a girl, medium full shot,
girl looking up toward the camera, visible floor,
natural perspective, soft lighting

1girl, black hair, red eyes, dark jacket, sitting on the floor,
looking up at the viewer
```

### H. Две девушки держатся за руки, разные внешности

```text
masterpiece, best quality, anime illustration,
2girls, full body, facing each other, holding hands,
hands meeting between them, looking at each other,
both characters fully visible, school courtyard

2girls, blonde hair, blue eyes, white blouse, blue pleated skirt,
standing on the left, facing right, holding the other girl's hands

2girls, black hair, red eyes, black jacket, red pleated skirt,
standing on the right, facing left, holding the blonde girl's hands
```

### I. Девушка ближе к камере, вторая позади

```text
masterpiece, best quality, anime illustration,
2girls, depth perspective, one girl in foreground,
one girl in background, both visible, medium wide shot

2girls, blonde hair, blue eyes, white blouse, standing in the foreground,
closer to the camera, looking back at the other girl

2girls, black hair, red eyes, black jacket, standing in the background,
farther from the camera, looking toward the blonde girl
```

### J. Вид из-за плеча: разговор

```text
masterpiece, best quality, anime illustration,
over-the-shoulder shot, conversation, eye level,
foreground shoulder visible, second character's face clearly visible,
soft indoor lighting

2girls, black hair, red eyes, seen from behind in the foreground,
dark jacket, shoulder close to the camera

2girls, blonde hair, blue eyes, facing the first girl,
white blouse, expressive face, looking at her
```

### K. Девушка сидит выше, другая стоит ниже

```text
masterpiece, best quality, anime illustration,
2girls, different vertical levels, one sitting on a stair,
one standing on a lower step, looking at each other,
full body, clear spatial relationship, stairwell

2girls, blonde hair, blue eyes, sitting on a higher step,
white blouse, blue skirt, looking down at the other girl

2girls, black hair, red eyes, standing on a lower step,
black jacket, red skirt, looking up at the blonde girl
```

### L. Диагональная композиция с Mask/Advanced

Общий промпт:

```text
masterpiece, best quality, anime illustration,
2girls, diagonal composition, dynamic pose,
both characters visible, indoor scene, soft cinematic light
```

Регион 1: девушка в верхней/левой части; регион 2: девушка в нижней/правой части. Для такой сцены Mask или Advanced обычно подходят лучше, чем Basic с жёстким делением пополам.

---

## 11. ControlNet, OpenPose, референсы и Inpaint

### 11.1. Что выбрать под задачу

| Задача | Начни с | Почему |
|---|---|---|
| Разная одежда и волосы у двух персонажей | Forge Couple | Разные региональные промпты |
| Положение слева/справа | Basic | Простое деление изображения |
| Нестандартная расстановка фигур | Advanced или Mask | Можно вручную назначить зоны |
| Точный наклон корпуса и положение рук | OpenPose/ControlNet или референс позы | Текстом трудно удержать скелет |
| Нужен конкретный угол камеры | Референс композиции + текст | Текст описывает ракурс, референс показывает геометрию |
| Исправить только кисть/лицо/одежду | Inpaint | Не требуется перегенерировать весь кадр |
| Одна и та же героиня в разных сценах | Character reference/LoRA при наличии | Теги сами по себе не фиксируют личность |
| Улучшить детализацию после удачной композиции | Hires.fix / upscale | Сначала композиция, потом детализация |

ControlNet добавляет к тексту условие, например карту позы или глубины. OpenPose помогает задать скелет тела; он не гарантирует конкретную личность, одежду или безупречные пальцы. Используй модель ControlNet, совместимую с SDXL/Forge и твоей конфигурацией: модели для SD 1.5 нельзя автоматически считать подходящими для SDXL.

### 11.2. Порядок работы с OpenPose

1. Найди/создай референс с нужной позой двух фигур.
2. В ControlNet выбери совместимый OpenPose-процессор/модель, если они установлены.
3. Проверь, что карта позы показывает обе фигуры и нужные суставы.
4. Сначала используй умеренную силу влияния, затем повышай её, если поза теряется.
5. Оставь Forge Couple для внешности и одежды.
6. После получения правильной позы исправь кисти через Inpaint, если нужно.

Не существует одного универсального веса ControlNet для всех картинок и моделей. Сила, начало/конец воздействия и препроцессор зависят от установленной модели и конкретного референса.

### 11.3. Inpaint — исправляй локально

Практический порядок:

1. Выбери изображение с правильной общей композицией.
2. Открой Inpaint и замаскируй только проблемную область.
3. Оставь достаточный запас вокруг дефекта, чтобы модель понимала контекст.
4. В промпте опиши только желаемую деталь и её связь с окружающими частями.
5. Не меняй seed/настройки сразу все вместе: сравни результат.
6. Если исправление портит окружающую область, уменьши маску или силу изменения и попробуй снова.

Пример для кистей, которые должны держаться:

```text
two natural hands holding each other,
clear hand contact, correct finger anatomy,
natural wrist alignment, consistent lighting
```

Негативный промпт может включать `extra fingers, fused fingers, malformed hands`, но длинный список отрицаний не гарантирует анатомию.

---

## 12. Настройки генерации и рабочий порядок

Для первого теста WAI Illustrious SDXL v1.7 можно использовать стартовые значения из проекта:

- Sampler: `Euler a`;
- Steps: `25` (опубликованный диапазон WAI: `15–30`);
- CFG: `6` (опубликованный диапазон WAI: `5–7`);
- Seed: фиксированный;
- Hires.fix: выключен для первого теста;
- разрешение: `1024 × 1344` для вертикальной сцены или `1344 × 1024` для горизонтальной сцены, если видеопамять позволяет.

Эти размеры соответствуют масштабу опубликованных примеров WAI. Если не хватает VRAM, временно уменьши разрешение для теста, но учитывай, что это уже компромисс, а не точное повторение рекомендаций карточки модели.

Это стартовые значения, а не обязательные. Важнее менять один параметр за раз. Для двух персонажей в полный рост часто удобно горизонтальное разрешение, потому что персонажам нужно пространство по ширине; вертикальное удобнее, когда кадр высокий и персонажи близко к камере. Сравнивай в одном и том же размере.

### Рабочая последовательность

1. Сначала проверь композицию без сложного регионального контроля.
2. Если персонажи смешиваются — включи Forge Couple и задай отдельные строки.
3. Если позиция/поза неверна — добавь OpenPose или референс.
4. Если один элемент неудачен — Inpaint только его.
5. Когда композиция устраивает, увеличивай детализацию и разрешение.
6. Сохраняй удачный seed и параметры.

Не меняй одновременно prompt, seed, CFG, sampler и режим регионов — иначе будет непонятно, что помогло.

---

## 13. Диагностика ошибок

### Одежда смешалась между персонажами
- Укороти региональные строки.
- Дай каждому 2–4 устойчивых отличительных признака.
- Проверь, что каждая строка находится в правильной области.
- Не помещай одежду одной героини в общий Global Effect.

### Высокая девушка получилась низкой
- Повтори рост в её регионе: `tall girl, noticeably taller`.
- Добавь контраст: `short girl, looking up` для второй.
- Проверь, не перепутаны ли регионы.
- Если важен рост на уровне силуэта, используй полный рост и контроль позы/референс.

### Камера смотрит не с той стороны
- Удали противоположные теги.
- Раздели камеру и направление тела: `side view` + `facing left`.
- Используй референс, если точный ракурс важнее свободы модели.

### Девушка стоит далеко, хотя нужна рядом
- Удали `wide shot`, `far away`, `small figure in the distance`.
- Добавь `close to viewer`, `standing very close`, `upper body`.
- Если требуется контакт, назови точку касания: `her head resting on the viewer's shoulder`.

### Персонаж прижимается, но контакт выглядит странно
- Описывай одну простую позу, не пять действий сразу.
- Укажи, какая часть тела касается другой.
- Сначала получи тела/силуэты, затем исправь кисти Inpaint.
- Для сложной позы используй OpenPose или референс.

### Лица или тела сливаются
- Добавь `clear separation between characters`, `distinct silhouettes`.
- Перемести персонажей немного дальше друг от друга, если контакт не обязателен.
- В Advanced/Mask проверь расположение и перекрытие регионов.
- Не ожидай, что региональные границы будут жёсткими контурами.

### Появился лишний персонаж
- Явно укажи `2girls` в каждой региональной строке.
- Удали неоднозначные слова, похожие на описание третьей фигуры.
- Используй негативный тег `extra person` только если проблема повторяется.

### После Hires.fix пропало разделение
- Проверь Compatibility в Forge Couple.
- Для диагностики временно отключи Hires.fix.
- Помни, что часть региональных настроек может не переноситься автоматически между вкладками; проверь актуальную инструкцию расширения.

---

## 14. Словарь английских тегов

### Камера и кадр
- `POV`, `first-person view` — от первого лица.
- `over-the-shoulder` — из-за плеча.
- `front view` — спереди.
- `side view`, `profile` — сбоку, профиль.
- `three-quarter view` — три четверти.
- `from behind`, `back view` — сзади.
- `eye level` — на уровне глаз.
- `low angle` — снизу вверх.
- `high angle` — сверху вниз.
- `bird's-eye view` — почти вертикально сверху.
- `worm's-eye view` — очень низкая камера.
- `close-up` — крупный план.
- `upper body` — верхняя часть тела.
- `cowboy shot` — примерно от середины бедра.
- `full body` — всё тело.
- `wide shot` — широкий план.
- `long shot` — дальний план.
- `foreshortening` — перспективное укорочение.

### Пространство
- `foreground` — передний план.
- `midground` — средний план.
- `background` — задний план.
- `in the distance`, `far away` — вдали.
- `close to viewer` — близко к зрителю.
- `on the left`, `on the right` — слева, справа.
- `centered composition` — по центру.
- `negative space` — свободное пространство.
- `depth perspective` — перспектива глубины.

### Поза и контакт
- `standing side by side` — стоят рядом.
- `standing face to face` — стоят лицом друг к другу.
- `looking at each other` — смотрят друг на друга.
- `holding hands` — держатся за руки.
- `hugging` — обнимаются.
- `side hug` — объятие сбоку.
- `leaning against` — опирается/прижимается к.
- `head resting on shoulder` — голова на плече.
- `arm around shoulders` — рука вокруг плеч.
- `arm around waist` — рука вокруг талии.
- `looking up at` — смотрит вверх на.
- `looking down at` — смотрит вниз на.
- `looking back over shoulder` — оглядывается через плечо.
- `natural body contact` — естественный контакт тел.
- `both characters fully visible` — оба персонажа полностью видны.
- `clear separation between characters` — персонажи визуально разделены.

### Рост и масштаб
- `tall girl` — высокая девушка.
- `short girl` — невысокая девушка.
- `petite` — миниатюрное телосложение.
- `long legs` — длинные ноги.
- `noticeably taller` — заметно выше.
- `noticeably shorter` — заметно ниже.
- `closer to camera` — ближе к камере.
- `farther from the camera` — дальше от камеры.
- `small figure in the distance` — маленькая фигура вдали.

---

## 15. План тестирования

Чтобы выяснить, какие формулировки лучше работают именно с твоей установкой:

1. Зафиксируй seed.
2. Сделай базовую картинку с одним персонажем или простой сценой.
3. Добавь только `side view` и сравни.
4. Верни базовый промпт и проверь только `high angle`.
5. Отдельно проверь `POV`, `close to viewer`, `full body`.
6. Затем включи Forge Couple и добавь вторую региональную строку.
7. Сохраняй изображения и параметры в таблицу: тег, seed, настройки, видимое отличие, побочные эффекты.
8. После того как композиция стабильно получается, добавляй детали одежды, свет и фон.

**Удобный принцип:** один эксперимент — одна основная переменная. Если поменять одновременно ракурс, позу, одежду и seed, нельзя надёжно понять причину результата.

---

## 16. Источники и ограничения

Основные источники, использованные для технических частей:

1. [Forge Couple — официальный README](https://github.com/Haoming02/sd-forge-couple) — Basic/Advanced/Mask, направления плиток, разделитель строк, Global Effect, ограничения и предупреждения.
2. [Forge Couple — CHANGELOG](https://github.com/Haoming02/sd-forge-couple/blob/main/CHANGELOG.md) — история изменений, включая ветку v7.x.
3. [ControlNet — официальный репозиторий](https://github.com/lllyasviel/ControlNet) — концепция управления генерацией дополнительными условиями, примеры позы и глубины.
4. [ControlNet OpenPose для SDXL на Hugging Face](https://huggingface.co/xinsir/controlnet-openpose-sdxl-1.0) — модель позового контроля для семейства SDXL.
   Не путай её с [lllyasviel/sd-controlnet-openpose](https://huggingface.co/lllyasviel/sd-controlnet-openpose): в карточке этой модели указан Stable Diffusion 1.5, поэтому это не тот же вариант для WAI Illustrious SDXL.
5. [WAI Illustrious SDXL v1.7.0 — карточка модели](https://huggingface.co/LyliaEngine/waiIllustriousSDXL_v170) — ориентир для параметров именно используемого чекпойнта.

### Как понимать этот справочник

- Английские теги — практические отправные точки, а не гарантированные команды.
- Один и тот же тег может вести себя по-разному из-за checkpoint, seed, sampler, CFG, размера, соседних тегов и регионального контроля.
- Не все фразы из словаря обязательно являются точными отдельными обучающими тегами.
- Документация Forge Couple объясняет устройство расширения, но не обещает точного результата для каждой композиции.
- Советы по ControlNet применимы только при наличии совместимых моделей и корректной конфигурации Forge.
- Если нужен точный результат, лучший путь — текст + региональный контроль + референс позы + локальный Inpaint, а не бесконечное удлинение одного промпта.

