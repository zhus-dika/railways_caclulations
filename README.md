# 🚇 Расчеты для КТЖ 

## 🐧 Необходимые данные (период: 2002-2024)

- Информация о грузоперевозках по типам грузов --> data/transportation_volumes_2002_2024.xlsx
- Информация об объеме производства --> data/volumes_2002_2024.xlsx
- Информация о тарифах --> data/2002-2024_transportation_volumes-tariffs.xlsx

## 🦜 Теоретическое описание моделей
- В этом исследовании использовались модели типа авторегрессионной модели с распределенными лагами (АРЛ), которая позволяет определить эффекты от изменения переменных, отвечающих за направление политики. Модели оценки эластичности спроса по цене имеют следующий вид:

  $$Y = a + b \cdot T + с \cdot X + d \cdot Yl + e \cdot S + Er,$$ 
  
  где   $Y$ – Индекс роста объемов перевозок грузов к соответствующему месяцу прошлого года
  
  $T$ – Индекс роста тарифов перевозок к предыдущему месяцу
  
  $X$ – Индекс роста объемов производства грузов к соответствующему месяцу прошлого года
  
  $Yl$ – Индекс роста объемов перевозок грузов к соответствующему месяцу прошлого года за предыдущий период (лаги)
  
  $S$ – Фиктивные переменные для учета скачков
  
  $Er$ – остатки модели, разница между фактическими данными и расчетными данными по модели
  
  $a$ – свободный коэффициент модели
  
  $b$, $c$, $d$, $e$ – коэффициенты регрессионной модели

  Стационарность временных рядов. Как известно, методы оценки уравнений множественной линейной регрессии, связывающих несколько экономических переменных, применимы только для стационарных рядов. 
  Кроме приведенных выше статистик качества модели существуют и другие статистики ее качества, позволяющие выбрать лучшую модель с помощью стандартных статистик и тестов на остатки модели, а именно:
  на выявление отсутствия автокорреляции остатков, т.е. независимость остатков от их значений за предыдущие периоды;
  Для лучшей оценки модели и построения прогнозов важно, чтобы остатки в модели были нормально распределены. Нормальность распределения остатков необходима для того, чтобы оценки коэффициентов модели являлись состоятельными, несмещенными и эффективными.
  на отсутствие гетероскедастичности остатков модели или постоянство дисперсий;
  
  Эластичность спроса по цене определяется по следующей формуле:
  
  $$E=\frac{b\cdot T}{a+b\cdot T},$$
  
  в случае $a=0$:
  
  $$E = b\cdot T.$$
                                                                                  
  Коэффициент $d$ определяет продолжительность эффекта повышения тарифов, т.е. восстановление спроса на перевозки после повышения тарифов и интерпретируется следующим образом: при снижении объемов перевозки на E% в период повышения тарифа, в следующем месяце объем снизится на $(1-E)\cdot 100$ процентов и составит $E\cdot d$.
  
  В диаграммах справа приведены качества построенных моделей. Т.е. показаны графики фактических объемов перевозок (Actual), расчетных объемов перевозок по построенной модели (Fitted) и разницы между ними (Residual).
  Как видно из диаграмм ошибка построенных моделей в пределах допустимой нормы.
  
  Источники данных: Объемы перевозок и динамика роста тарифов услуг МЖС КТЖ, объемы производства БНС в период 2002-2024 гг.


## 🦉 Расчеты в ноутбуках

- Загрузка и автоматическая обработка данных о производстве с ресурса https://stat.gov.kz/ru/  -->  railways_calculations_volume_data_p1_3.ipynb
- Загрузка данных и построение основных моделей для расчетов  -->  regression_arma_models_by_custom_interval.ipynb
  **Пояснения:**
  1. Данные разбиты по частоте (monthly - по месяцам, quarterly - квартальные, semi-annual - полугодовые, annual - годовые).
  2. Данные сглажены с помощью фильтра Hodrick–Prescott (можно использовать другой)
  3. Есть возможность подбирать параметры модели.
  4. Для Зерна объемы производства были разбиты равномерно по месяцам.
