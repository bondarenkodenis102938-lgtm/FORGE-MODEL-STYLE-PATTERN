# Positive и Negative промпты для анатомии и стиля тела
## Практический справочник для WAI Illustrious SDXL v1.7.0 в Stable Diffusion WebUI Forge

Этот справочник написан по-русски. Английские слова в блоках промптов оставлены на английском намеренно: WAI Illustrious — модель, обученная в том числе на тегах в стиле Danbooru, поэтому английские теги обычно удобнее для прямого тестирования.

**Главное ограничение:** тег — это подсказка модели, а не команда с гарантированным результатом. Ни один список не гарантирует идеальные руки, ноги или лицо. Влияние тегов зависит от чекпойнта, композиции, позы, seed, разрешения и соседних тегов.

## 1. Как читать этот гайд

- **Positive prompt** — что хотим получить: форму, видимую деталь, позу, стиль рендеринга.
- **Negative prompt** — что хотим ослабить: конкретную ошибку, например лишние пальцы или слипшиеся ноги.
- **База модели** — короткий negative, опубликованный в карточке WAI.
- **Практический тег** — идея для эксперимента; не обязательно официальный тег и не гарантированное исправление.
- **Стиль** — внешний вид рисунка: линия, заливка, тени и уровень детализации.
- **Анатомия** — форма и расположение частей тела. Хорошая анатомия и красивый стиль — разные задачи.

Не вставляй все блоки сразу. Сначала определи, что конкретно не так на изображении, и добавь только соответствующую группу.

## 2. Стартовая база для WAI Illustrious v1.7.0

Карточка модели показывает такой позитивный блок качества:

```text
masterpiece, best quality, amazing quality
```

И такой короткий negative:

```text
bad quality, worst quality, worst detail, sketch, censor
```

В карточке также указаны Euler a и CFG 5–7. Это стартовые рекомендации, а не обязательные настройки для каждой картинки.

**Для первых тестов не раздувай промпт.** Начни с базы модели и добавляй один небольшой блок по конкретной проблеме. Слишком много отрицаний и повторяющихся тегов может изменить изображение не так, как ожидалось.

## 3. Общая схема: что добавить, если видишь проблему

| Что не нравится | Сначала попробуй |
|---|---|
| В целом искажённое тело | `bad anatomy, bad proportions, deformed` |
| Лишняя рука или нога | `extra limbs, extra arms, extra legs` |
| Не хватает части тела | Проверь кадрирование и позу; попробуй `full body` или нужную позу |
| Пальцы слились | `fused fingers, malformed fingers` |
| Появились лишние пальцы | `extra fingers, too many fingers` |
| Глаза смотрят в разные стороны | `cross-eyed, asymmetrical eyes` |
| Стопы и пальцы ног искажены | `bad feet, deformed feet, malformed toes` |
| Тело выглядит слишком реалистично | Попробуй выбранный anime/style-блок, а не десятки анатомических отрицаний |
| Тело выглядит слишком плоско | Попробуй `soft shading`, `gradient shading` или `ambient occlusion` — по одному |
| Контур слишком толстый | Убери `thick outlines`, попробуй `delicate lineart` или `thin lineart` |

Negative не может надёжно дорисовать скрытую руку или ногу. Для устойчивых ошибок используй inpainting, позный контроль или более ясную композицию.

---

# Часть I. Анатомия по зонам

## 4. Лицо в целом

**Что описывают эти теги:** общую ясность черт лица. Они не задают конкретную форму глаз, носа или рта.

Positive — варианты для теста:

```text
detailed face, delicate facial features, expressive face
```

Negative — только если видишь такие ошибки:

```text
bad face, deformed face, distorted face
```

- `detailed face` — просит больше заметных деталей лица.
- `delicate facial features` — подталкивает к более тонким и мягким чертам; результат зависит от модели.
- `expressive face` — акцент на выразительности.
- `distorted face` — попытка ослабить искажение, но не точная команда на исправление.

Не добавляй `symmetrical face` автоматически: идеальная симметрия может сделать выражение менее естественным.

## 5. Глаза, зрачки и взгляд

Базовый positive:

```text
detailed eyes, expressive eyes
```

Если нужен взгляд прямо на зрителя, добавь:

```text
looking at viewer
```

Если взгляд должен быть направлен в сторону, используй вместо этого:

```text
looking away
```

Negative при конкретных ошибках:

```text
bad eyes, asymmetrical eyes, cross-eyed, malformed eyes
```

Дополнительные тесты:

- `detailed iris` — акцент на рисунке радужки.
- `visible pupils` — если зрачки теряются.
- `half-closed eyes` — для полуприкрытых век.
- `closed eyes` — если глаза должны быть закрыты.

Не смешивай противоположные состояния, например `closed eyes` и `looking at viewer`, если это не осознанный эксперимент. Тег `symmetrical eyes` может уменьшить различия между глазами, но иногда делает лицо менее живым.

## 6. Брови, нос, рот и уши

### Брови

Positive:

```text
expressive eyebrows, detailed eyebrows
```

Negative:

```text
malformed eyebrows, uneven eyebrows
```

Эти теги необязательны: если брови почти не видны, их влияние будет небольшим.

### Нос

Positive:

```text
delicate nose, defined nose
```

Negative:

```text
distorted nose, malformed nose
```

В аниме нос часто нарисован минимально. `defined nose` может сделать его заметнее и приблизить лицо к более реалистичной подаче.

### Рот и зубы

Positive — выбери нужное состояние:

```text
natural smile
```

```text
closed mouth
```

```text
parted lips
```

Negative:

```text
bad mouth, distorted mouth, malformed teeth, extra teeth
```

Не добавляй одновременно противоречащие состояния рта. `parted lips` — слегка разомкнутые губы; это не обязательно широкая улыбка или открытый рот.

### Уши

Positive:

```text
visible ears, detailed ears
```

Negative:

```text
bad ears, malformed ears, extra ears
```

`visible ears` используй, если уши должны быть видны из-под волос. Если они закрыты причёской, это не ошибка.

## 7. Волосы и линия роста волос

Positive для деталей:

```text
detailed hair, detailed hair strands, hair highlights
```

Для движения:

```text
flowing hair
```

Для слоистой причёски:

```text
layered hair
```

Negative — только если волосы выглядят неправильно:

```text
poorly drawn hair, tangled hair
```

Не используй `messy hair` как универсальный negative: это может противоречить желаемой растрёпанной или ветром развеваемой причёске.

## 8. Шея, плечи и ключицы

Positive:

```text
natural neck, natural shoulders
```

Для тонкой шеи как художественного решения:

```text
slender neck
```

Для видимых ключиц:

```text
defined collarbones
```

Negative:

```text
deformed neck, twisted neck, deformed shoulders
```

- `slender neck` меняет дизайн персонажа; это не универсальное исправление.
- `defined collarbones` добавляет акцент на ключицах и может быть нежелателен для мягкого, упрощённого стиля.
- Разная высота плеч может быть правильной, если персонаж наклонён или повернут.

## 9. Туловище, грудная клетка, талия и спина

Базовый positive:

```text
natural torso, balanced proportions
```

Для более заметного силуэта:

```text
defined waist
```

Negative при искажениях:

```text
bad anatomy, deformed torso, twisted torso, bad proportions
```

`defined waist` — эстетический выбор, а не просто повышение анатомической точности. Не складывай одновременно несовместимые описания пропорций. Если спина выглядит странно из-за сложной позы, попробуй сначала упростить позу.

## 10. Руки, локти и предплечья

Positive:

```text
natural arms, natural arm proportions
```

Если руки должны быть заметны в кадре, уточни действие:

```text
arms crossed
```

или

```text
one arm raised
```

Negative при конкретных дефектах:

```text
bad arms, deformed arms, twisted arms, extra arms, disconnected limbs
```

- `extra arms` имеет смысл, если действительно появилась лишняя рука.
- `twisted arms` — экспериментальный тег, а не надёжная анатомическая команда.
- Чёткое описание позы часто полезнее длинного списка отрицаний.

## 11. Кисти и пальцы рук

### Общая форма кисти

Positive:

```text
detailed hands, natural hands
```

Negative:

```text
bad hands, deformed hands, mutated hands
```

### Количество и разделение пальцев

Positive для теста:

```text
five fingers, separated fingers, visible thumb
```

Negative:

```text
extra fingers, missing fingers, fused fingers, malformed fingers
```

Это подсказки, а не гарантия правильного количества пальцев. Если кисть маленькая в кадре, модель может не прорисовать каждый палец независимо от промпта.

### Положение рук

Выбирай подходящий тег действия:

```text
open hand
```

```text
relaxed hands
```

```text
hands clasped
```

```text
hand on hip
```

Negative при видимых ошибках:

```text
floating hands, disconnected hands, fused hands
```

Если рука держит предмет, назови предмет и действие: например, `right hand holding a cup`. Не перегружай запрос десятком разных поз кисти.

## 12. Таз, бёдра, колени и ноги

### Таз и бёдра

Positive:

```text
natural hips, balanced proportions
```

Negative:

```text
deformed hips, twisted pelvis, unnatural proportions
```

### Ноги в целом

Positive:

```text
natural legs, natural leg proportions
```

Negative:

```text
bad legs, deformed legs, extra legs, fused legs, twisted legs
```

### Колени и голени

Positive для теста:

```text
natural knees, natural leg proportions
```

Negative:

```text
malformed knees, deformed legs
```

Теги `defined thighs` или `slender legs` меняют внешний вид тела. Не считай их универсальными тегами исправления.

Если одна нога находится позади другой, опиши положение словами, например `one leg forward, one leg behind`. Отрицание `fused legs` не всегда решит проблему сложного перекрытия.

## 13. Стопы, пальцы ног и обувь

Если босые стопы видны крупно:

```text
detailed feet, natural feet, visible toes
```

Negative при ошибках:

```text
bad feet, deformed feet, malformed toes, extra toes, fused toes
```

Если персонаж в обуви, описывай обувь, а не пальцы ног. Для маленьких стоп на дальнем плане подробные теги могут почти не влиять на результат.

## 14. Общие теги анатомии

Positive — пробуй по одному:

```text
good anatomy, balanced proportions, natural pose
```

Negative — короткий блок для заметной общей проблемы:

```text
bad anatomy, bad proportions, deformed, extra limbs, disconnected limbs
```

Не гарантируется, что `good anatomy` улучшит любую картинку: это широкое описание. Если дефект только на одной кисти, начни с блока для кистей, а не добавляй все анатомические отрицания.

---

# Часть II. Художественные стили для разных частей тела

## 15. Сначала важное о стиле

Теги ниже управляют преимущественно **подачей рисунка**, а не правильностью анатомии. Они могут менять не только одну руку или одну ногу, но и всю картинку.

Например, `soft shading` может смягчить тени на теле в целом. Если написать в одном промпте `cel shading` для рук и `painterly rendering` для ног, модель может не разделить стили по частям так чётко, как задумано. Для согласованного результата проще выбрать один общий стиль, а локальные особенности задавать описанием детали.

### Стиль A — чистое аниме с тонкими линиями

```text
clean lineart, thin lineart, cel shading, smooth colors
```

Что ожидать: контуры заметны, формы читаются быстро, тени обычно выглядят более графичными и разделёнными. Подходит для аккуратных персонажей, чистого силуэта, ясного разделения пальцев и складок одежды.

Возможный минус: переходы света могут быть резкими, а объём — менее живописным.

### Стиль B — мягкая полуживописная иллюстрация

```text
delicate lineart, soft shading, gradient shading, subtle brush texture
```

Что ожидать: мягкие тени и плавные цветовые переходы, контур не должен перетягивать всё внимание. Хороший старт, если хочется аккуратную аниме-картинку с мягким объёмом.

Возможный минус: при слишком мягкой подаче пальцы и суставы могут стать менее чёткими.

### Стиль C — живописная подача

```text
painterly rendering, soft brushwork, subtle texture, rich midtones
```

Что ожидать: ощущение мазков и более живописного света, менее механически ровные поверхности.

Возможный минус: чёткость мелких анатомических деталей может снизиться. Не добавляй много слов про текстуру, если хочешь гладкую кожу.

### Стиль D — объёмное мягкое освещение

```text
soft lighting, soft cast shadows, ambient occlusion, smooth shading
```

Что ожидать: более ясное ощущение объёма за счёт теней и мягкого света.

Возможный минус: `ambient occlusion` иногда делает контактные тени слишком сильными. Проверяй его отдельно.

**Для твоего желаемого направления начни с B**, затем отдельно сравни B + `warm lighting` и B + `subtle brush texture`. Не добавляй все варианты сразу.

## 16. Стили рук и кистей

### Вариант 1 — чистая, аккуратная кисть

```text
detailed hands, clean lineart, thin lineart, cel shading, separated fingers
```

Подходит, когда хочется ясного контура кисти и хорошо разделённых пальцев. `separated fingers` описывает визуальное разделение, но не гарантирует правильную анатомию.

### Вариант 2 — мягкие изящные руки

```text
delicate hands, delicate lineart, soft shading, gradient shading
```

Подходит для мягкой, полуживописной иллюстрации. Слово `delicate` может влиять на тонкость и общее впечатление от формы; это эстетическая подсказка, а не медицинское описание анатомии.

### Вариант 3 — более выразительные и детализированные кисти

```text
detailed hands, defined fingers, subtle shadows, ambient occlusion
```

Подходит, если кисть крупная в кадре и должна быть визуально понятной. Если тени между пальцами становятся слишком тёмными, убери `ambient occlusion`.

### Вариант 4 — гладкая стилизованная аниме-кисть

```text
anime style, smooth shading, clean lineart, simplified details
```

Подходит для упрощённой графики. Это скорее стиль, чем способ исправления ошибок. Если пальцы теряются, верни `detailed hands` и проверь результат.

## 17. Стили рук, предплечий и плеч

### Тонкие и изящные руки

```text
slender arms, delicate lineart, soft shading, smooth skin shading
```

Что меняет: общий силуэт рук стремится выглядеть тоньше и мягче. Используй только если это соответствует дизайну персонажа.

### Более чёткие формы и объём

```text
defined arms, clean lineart, cel shading, clear cast shadows
```

Что меняет: форма руки становится более графичной, переходы света могут быть резче.

### Мягкая полуживописная анатомия

```text
natural arm proportions, soft shading, gradient shading, subtle brush texture
```

Что меняет: меньше жёстких границ в тенях, мягче объём.

Negative для проблем с руками — не добавляй все варианты автоматически:

```text
bad arms, deformed arms, extra arms, bad hands, extra fingers, fused fingers
```

Если испорчена только кисть, начни с `bad hands, extra fingers, fused fingers`; если проблема выше локтя, тестируй `bad arms, deformed arms`.

## 18. Стили ног и коленей

### Длинный, тонкий силуэт

```text
long legs, slender legs, clean lineart, soft shading
```

Это эстетический выбор: модель может изменить пропорции ног. Используй, если именно такой силуэт нужен.

### Чёткая аниме-форма

```text
natural leg proportions, clean lineart, cel shading, defined shadows
```

Подходит для более графичного стиля, где форма коленей и ног читается через линии и тени.

### Мягкие полуживописные ноги

```text
natural leg proportions, gradient shading, soft cast shadows, smooth shading
```

Подходит для плавных переходов и мягкого объёма.

### Более спортивная форма

```text
athletic legs, defined muscles, natural leg proportions, directional lighting
```

Используй, только если хочешь выраженную мускулатуру. Эти слова меняют дизайн тела и не являются универсальным улучшением.

Negative при реальных ошибках:

```text
bad legs, deformed legs, fused legs, extra legs, malformed knees
```

## 19. Стили стоп

### Простые стилизованные стопы

```text
anime style, clean lineart, smooth shading, simplified details
```

Подходит, если стопы маленькие в кадре и не должны выглядеть фотореалистично.

### Более детализированные босые стопы

```text
detailed feet, natural feet, soft shading, subtle shadows
```

Подходит для крупного плана босых стоп. Теги не гарантируют правильное количество пальцев.

### Чёткая графическая стопа

```text
detailed feet, clean lineart, cel shading, defined contours
```

Подходит для рисунка с выраженным контуром.

Negative при ошибках:

```text
bad feet, deformed feet, malformed toes, extra toes, fused toes
```

Если стопы скрыты обувью, лучше описать обувь и её форму. Не проси одновременно видимые пальцы ног и закрытые ботинки.

## 20. Стили туловища и кожи

### Мягкая гладкая кожа

```text
smooth skin shading, soft shading, gradient shading, soft lighting
```

Ожидаемый результат: плавные тени и меньше резких переходов.

### Чистая графическая анатомия

```text
clean lineart, cel shading, defined contours, clear shadows
```

Ожидаемый результат: более чёткая форма, графическое разделение света и тени.

### Живописный объём

```text
painterly rendering, soft brushwork, rich midtones, subtle texture
```

Ожидаемый результат: мягкий живописный рендер и более заметная фактура.

### Более реалистичные объёмные формы

```text
natural body proportions, soft cast shadows, ambient occlusion, directional lighting
```

Это может приблизить освещение и формы к более объёмной подаче, но не гарантирует фотореализм. Если результат стал слишком тёмным, убери `ambient occlusion` или `directional lighting` и сравни.

## 21. Можно ли каждой конечности назначить отдельный стиль?

Можно попробовать, но результат не всегда предсказуем. Модель может применить стиль ко всему персонажу вместо выбранной конечности.

Пример единого стиля с разной степенью детализации:

```text
semi-painterly anime illustration, delicate lineart, soft shading, gradient shading,
detailed hands, detailed fingers, natural arm proportions, natural leg proportions,
detailed feet, subtle brush texture
```

Здесь общий стиль единый, а руки и стопы получают отдельные подсказки на детализацию.

Если нужна действительно другая прорисовка только одной кисти или стопы, надёжнее:
1. Сгенерировать картинку в одном стиле.
2. Использовать inpainting на конкретной области.
3. Менять только теги выбранной области и сохранять остальной промпт.
4. Проверить, не выбивается ли отредактированная часть из общего стиля.

Не пытайся одновременно получить `cel shading` и `painterly rendering` на одной и той же области, если не хочешь смешанный результат.

---

# Часть III. Готовые блоки для копирования

## 22. Базовый WAI + мягкий аниме-стиль

Positive:

```text
masterpiece, best quality, amazing quality, 1girl, solo,
semi-painterly anime illustration, delicate lineart, soft shading,
gradient shading, soft lighting, detailed eyes, detailed hair
```

Negative:

```text
bad quality, worst quality, worst detail, sketch, censor
```

Это стартовый шаблон. Добавь внешность, одежду, позу и фон отдельно.

## 23. База + руки и пальцы

Добавь в positive, если кисти хорошо видны:

```text
detailed hands, natural hands, separated fingers
```

Добавь в negative, только если есть соответствующие ошибки:

```text
bad hands, deformed hands, extra fingers, fused fingers, malformed fingers
```

## 24. База + ноги и стопы

Positive:

```text
natural leg proportions, natural pose
```

Если стопы босые и крупно видны:

```text
detailed feet, natural feet
```

Negative при проблемах:

```text
bad legs, deformed legs, fused legs, bad feet, deformed feet, malformed toes
```

## 25. База + общая анатомия

Negative:

```text
bad anatomy, bad proportions, deformed, extra limbs, disconnected limbs
```

Не добавляй этот блок, если тело уже выглядит хорошо. Не существует обязательного списка, который нужен каждой генерации.

---

# Часть IV. Как проверить теги в Forge

## 26. Простой эксперимент без путаницы

1. Выбери один сюжет, один checkpoint и один seed.
2. Оставь одинаковыми sampler, steps, CFG и разрешение.
3. Сгенерируй картинку с коротким промптом.
4. Добавь только один блок, например `detailed hands, separated fingers`.
5. Сравни именно руки, а не только общее впечатление.
6. Запиши, стало ли лучше, хуже или ничего не изменилось.
7. Удали теги, которые не помогают.

Не меняй одновременно стиль, анатомию, освещение и композицию: иначе будет трудно понять, какой тег повлиял на результат.

## 27. Шаблон записи эксперимента

| Поле | Что записать |
|---|---|
| Модель | WAI Illustrious SDXL v1.7.0 |
| Seed | Одно и то же значение в сравнении |
| Проблема | Например, пальцы сливаются |
| Добавленный тег | Например, `separated fingers` |
| Negative | Например, `fused fingers` |
| Результат | Лучше / хуже / без заметной разницы |
| Решение | Оставить / удалить / проверить отдельно |

## 28. Что официально подтверждено, а что нет

- **Подтверждено карточкой WAI:** короткий quality-блок, короткий negative-блок, рекомендуемые sampler и диапазон CFG.
- **Практические варианты:** большинство тегов для анатомии и стиля в этом документе. Это подсказки для экспериментов, а не официальные гарантии автора модели.
- **Не гарантируется:** исправление любой ошибки одним тегом, одинаковое действие во всех позах и независимое применение стиля только к одной конечности.

## Источники

1. Карточка модели WAI Illustrious SDXL v1.7.0: https://huggingface.co/LyliaEngine/waiIllustriousSDXL_v170
2. Руководство по промптам для Illustrious / NoobAI / Pony: https://github.com/tsuseki/vrfu-ai/blob/main/docs/prompting.md
3. MonAI — WAI Illustrious v17.0: https://wiki.monai.art/en/models/wai_illustrious_17

Для тегов, которых нет в карточке модели, проверяй эффект на своём seed. Не считай популярность тега доказательством, что он обязательно поможет именно твоему checkpoint.
