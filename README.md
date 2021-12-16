# module_7
 [SF-DST] Car Price prediction  
   
Признаки  
   
Особо несбалансированные признаки:    
• driveSide - правосторонни машины в большинстве => поисследовать, есть ли левосторонние машины в тесте. Если нет, то удалить признак    
• bodyType - некоторые категории представлены небольшим количеством машин, но boxplot показывает значительные различия и распределение по ценам. Идея схлопнуть малочисленные категории в other кажется не очень хорошей, т.к. разброс медианных цен очень большой для этих категорий. Оставлим пока признак, как есть.  
• brand - много категорий, заметно, что есть массовые, среднепопулярные и редкие бренды авто. Поисследовать дополнительно и подумать над созданием новых признаков (престижные авто/люкс, популярные и т.д.)  
• color - есть популярные цвета (черный, белый, серый, серебристый, синий) и редкие. Посмотреть дополнительно и создать новый признак о популярности цвета  
• fuelType - есть типы топлива, которые в явном меньшинстве. Поисследовть и подумать, стоит ли делать группировку непопулярных типов топлива  
• tcp - несбалансированный признак, но пока оставляем в модели  
• model - очень много уникальных категорий, подумать, можно ли как-то доработать признак. Подумать про объединение brand + model  
• numberOfDoors - малое количество машин в 0-3 - изучить детальнее  
Сбалансированные признаки с заметно превалирующим классом:  
• ownershipTimeIsNull - большая часть записей без указания времени владения  
• transmission - автомат превалирует  
• drivertrain - передний привод встречается чаще всего  
• vendor - большее количество автомобилей европейского региона  
• ownersCount - привалирует 3 и более.  
Неинформативные признаки: conditions, customs - после манипуляций с данными в признаках осталось только одно значение. Удаляем из анализа.  
Зависимость с целевой переменной:  
• ownershipTimeIsNull: машины без указания времени владения в среднем дешевле, чем машины с указанием времени владения  
• driveSide: авто с правосторонним рулем в среднем дешевле машин с левосторонним рулем  
• transmission: авто с АТ коробкой намного дороже MT, как и сам диапазон цен  
• bodyType: признак, который значительно влияет на распределение цен  
• brand: большая разбежка цен от бренда. Выделяются престижные авто (porche, Cadillac, bmw, and Rover, Lexus и др), а есть дешевый сегмент (азиатские авто - Cherry, Daewoo, Great wall и др.). Также видны бренды, которые выпускают дорогие авто, но и есть модели для более дешевого сегмента.  
• color: цены зависят от цвета, но большие цены представлены у цветов, количество авто по которым больше.  
• fuelType: очень дорогие машины электро и дизель, возможно выделить отдельный признак, что машина “электрокар”  
• drivetrain: полноприводные машины дороже всех, заднеприводные машины в среднем дешевле переднеприводных  
• tcp авто с дубликатом ПТС дешевле чем те, что с оригиналом  
• model - данных много, но видно, что присутствую колебания цены в зависимости от модели  
• vendor: в среднем, европейские и японские машины дороже американских и азиатских  
• vehicleTransmission: в среднем разновидности автоматов особо не влияют на цену, проверить значимость признака тестом Стьюдента. Потенциально на исключение.  
• numberOfDoors: в среднем самые дорогие авто - 2-х дверные, затем 5-дверные.  
• ownersCount: чем больше владельцев, тем ниже средняя цена авто.  
   
Новые признаки:   
• km_per_year - показывает, сколько км в год проезжал автомобиль.
• carNovelty - показывает, через сколько лет после выхода модели был выпущен автомобиль  
• prodDate_3Y - признак, что автомобилю уже 3 года  
• prodDate_5Y - признак, что автомобилю уже 5 лет  
• newCar - признак для обозначения новых авто, пробег равен 0  
• descriptionIsNull - признак, показывающий наличие или отсутствие описания  
• ownershipTimeIsNull - показывает, заполнено ли время владения авто: 1 - не заполнено, 0 - заполнено  
• brandPopular - признак для обозначения, что авто популярной марки: 1 - популярного, 0 - непопулярного  
• modelPopular - признак для обозначения, что авто популярной модели в рамках бренда: 1 - популярного, 0 - непопулярного  
  
Результаты моделирования:  
Модель №2: CatBoost  
Точность модели по метрике MAPE без логтаргета: 15.07%  
Точность модели по метрике MAPE с логтаргетом: 12.74%  
  
Модель 3: RandomForestRegressor    
Точность модели по метрике MAPE без логтаргета: 14.97%    
Точность модели по метрике MAPE с логтаргетом: 13.40%  
Точность модели по метрике MAPE с логтаргетом и гиперпараметрами: 13.35%  
  
Модель 4: XGBRegressor  
Точность модели по метрике MAPE без логтаргета: 14.62%  
Точность модели по метрике MAPE с логтаргетом: 12.69%  
Точность модели по метрике MAPE с логтаргетом и max_depth=8: 12.63%  
  
Модель 5: ExtraTreesRegressor  
Точность модели по метрике MAPE без логтаргета: 15.03%  
Точность модели по метрике MAPE с логтаргетом: 13.63%  
Точность модели по метрике MAPE с логтаргетом и гиперпараметрами: 13.27%  
  
Модель 6: BaggingRegressor  
RandomForest   
Точность модели по метрике MAPE с логтаргетом: 13.35%    
  
ExtraTreesRegressor  
Точность модели по метрике MAPE с логтаргетом: 13.51%  
  
Модель 7. StackingRegressor  
Xgboosting, ExtraTreesRegressor+LinearRegression  
Точность модели по метрике MAPE: 12.62%  
  
Xgboosting, ExtraTreesRegressor+CatBoostRegressor  
Точность модели по метрике MAPE: 12.85%  
  
RandomForestRegressor, ExtraTreesRegressor+LinearRegression  
Точность модели по метрике MAPE: 13.18%  
  
BaggingRegressor(xgb.XGBRegressor), ExtraTreesRegressor+LinearRegression  
Точность модели по метрике MAPE: 12.59%  
  
BaggingRegressor(ExtraTreesRegressor), ExtraTreesRegressor+LinearRegression  
Точность модели по метрике MAPE: 13.32%  
  
Лучшая модель:  
BaggingRegressor: ExtraTreesRegressor+LinearRegression  
Точность модели по метрике MAPE: 12.59%  
