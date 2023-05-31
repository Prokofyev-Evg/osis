# Описание протокола OSIS
- - - -
## Общая информация
Протокол необходим для получения подробной информации о турнире в реальном времени. Данные поступают в пакетном режиме в виде сообщений с гиперразметкой. 

## Подключение к ISUCalcFS
Подключение к ISUCalcFS осуществляется по протоколу TCP/IP. По умолчанию, из коробки ISUCalcFS настроен в качестве сервера на прием входящих соединений по 4000-му порту. Приложение может также работать в качестве клиента и на другом порту, эту настройку можно поменять на необходимые пользователю параметры в файле конфигурации ~ISUCalcFS.ini~, который находится в корневой папке с программой.

## Сообщения
Сообщения разделяются спецсимволами SOF(`0x02`) и EOF(`0x03`), обозначающими начало и конец сообщения. Каждое сообщение представляет из себя гиперразметку. Все сообщения имеют родительский тег `Isu_Osis`, полезным в нем будет атрибут `ISUCalcFS` который содержит версию программного обеспечения. Дочерние же элементы отличаются, и зависят от события.

### Event_Overview
Сообщение **Event_Overview** отправляется в момент запуска соревнования и содержит в себе описание всего турнира, название, разряды и виды которые представлены в турнире. Структура тега:
```xml
<Isu_Osis>
    <Event_Overview>
        <Event>
            <Category_List>
                <Category>
                    <Segment_List>
                        <Segment></Segment>
                        .
                        .
                        .
                    </Segment_List>
                </Category>
                .
                .
                .
            </Category_List>
        </Event>
    </Event_Overview>
</Isu_Osis>
```

#### Атрибуты тега `Event_Overview`
* **ID** - Внутренний индекс турнира
* **Name** - Название турнира
* **ExtDt** - Внешний индекс турнира

#### Атрибуты тега `Category`
* **ID** - Внутренний индекс разряда
* **Name** - Название разряда
* **ExtDt** - Внешний индекс разряда
* **Gender** - Пол спортсменов F-Female/M-Male
* **Type** - Вид разряда SGL-одиночники, PRS-Пары

#### Атрибуты тега `Segment`
* **ID** - Внутренний индекс сегмента
* **Name** - Название сегмента
* **Abbreviation** - Аббревиатура названия сегмента
* **ExtDt** - Внешний индекс сегмента
* **Date** - Дата проведения сегмента
* **Start** - Время запланированного начала сегмента
* **End** - Время запланированного окончания сегмента
* **Status** - статус турнира. Ожидается - c, завершен - g

#### Пример

``````xml
<Isu_Osis Part="1" version="2.0" Counter="1" IsuCalcFs="3.7.3" Database="11149">
	<Event_Overview>
		<Event ID="1" Name="Children of Asia 2023" Abbreviation="CoA2023" Type="T" CmpType="Y" ExtDt="000000000145">
			<Category_List>
				<Category ID="1" Tec_Id="0" Name="Women" Level="JUN" Gender="F" Type="SGL" ExtDt="000000000150" TypeName="Women">
					<Segment_List>
						<Segment ID="1" Name="Short Program" Abbreviation="SP" Date="20230226" Start="15:45:00" Status="g" End="18:53:00" ExtDt="000000000151"/>
						<Segment ID="4" Name="Free Skating" Abbreviation="FS" Date="20230228" Start="16:00:00" Status="c" End="19:30:00" ExtDt="000000000152"/>
					</Segment_List>
				</Category>
				<Category ID="2" Tec_Id="0" Name="Men" Level="JUN" Gender="M" Type="SGL" ExtDt="000000000151" TypeName="Men">
					<Segment_List>
						<Segment ID="2" Name="Short Program" Abbreviation="SP" Date="20230226" Start="13:00:00" Status="g" End="15:08:15" ExtDt="000000000152"/>
						<Segment ID="3" Name="Free Skating" Abbreviation="FS" Date="20230227" Start="17:00:00" Status="c" End="19:15:45" ExtDt="000000000154"/>
					</Segment_List>
				</Category>
			</Category_List>
		</Event>
	</Event_Overview>
</Isu_Osis>

``````

### Segment_Start
Сразу после `Event_Overview` отправляется сообщение `Segment_Start`. Сообщение содержит подробную информацию о запущенном разряде: Главная судейская коллегия, судьи, обслуживающие данный вид соревнований, участники, клубы, тренеры, все параметры сегмента, стартовый порядок участников и их расположение по группам разминок. Структура тега:

``````xml
<Isu_Osis>
	<Segment_Start>
		<Event>
		<Event_Officials_List>
			<Official>
                .
                .
                .
		<Event_Officials_List/>
			<Category_List>
				<Category>
					<Participant_List>
						<Participant>
							<Athlete_List>
								<Athlete/>
							</Athlete_List>
						</Participant>
						.
						.
						.
					</Participant_List>
					<Category_Result_List>
						<Participant/>
						.
						.
						.
					</Category_Result_List>
					<Segment_List>
						<Segment>
							<Criteria_List>
								<Criteria/>
								.
								.
								.
							</Criteria_List>
							<Deduction_List>
								<Deduction/>
								.
								.
								.
							</Deduction_List>
							<Majority_Deduction_List>
								<Majority_Deduction/>
								.
								.
								.
							</Majority_Deduction_List>
							<Segment_Official_List>
								<Official/>
								.
								.
								.
							</Segment_Official_List>
							<Segment_Start_List>
								<Performance>
									<Planned_Element_List>
										<Element/>
										.
										.
										.
									</Planned_Element_List>
								</Performance>
								.
								.
								.
							</Segment_Start_List>
							<Warmup_Group_List>
								<Warmup_Group/>
								.
								.
								.
							</Warmup_Group_List>
						</Segment>
						.
						.
						.
					</Segment_List>
				</Category>
			</Category_List>
		</Event>
	</Segment_Start>
</Isu_Osis>

``````

#### Атрибуты тега `Segment_Start`

* **Category_ID** - Внутренний индекс запущенного разряда
* **Segment_ID** - Внутренний индекс запущенного сегмента
* **Segment_Index** - Индекс сегмента, по сути порядковый номер сегмента в категории
* **Starting_Participant** - Внутренний индекс спортсмена с которого запустили соревнование
* **Next_Participant** - Внутренний индекс следующего спортсмена
* **LastFinished_Participant** - Внутренний индекс последнего посчитанного спортсмена

#### Атрибуты тега `Event`, `Category` и `Segment` 

Аналогично тегам внутри `Event_Overview`

#### Атрибуты тега `Participant` внутри `Participant_List`

* **ID** - Внутренний индекс турнира
* **Name** - Название турнира
* **ExtDt** - Внешний индекс турнира

#### 

#### Атрибуты тега `Segment_Start`

### Segment_Running

Данный тип пакета приходит на каждое действие в ISUCalcFS во время запущенного соревнования. Тип действия содержится в атрибуте `Command` дочернего тега `Action`. Сам запуск соревнования не является исключением. При запуске соревнования этот пакет присылается со значением `INI` в поле `Command`. Соседними будут теги `Category_Result_List` с текущими результатами категори и `Segment_Start_List` со стартовым порядком спортсменов.

1S1 - Подробная детализация выступления спортсмена
1S2 - Таблица результатов сегмента
1S3 - Таблица результатов разряда
1S4 - Стартовая таблица сегмента
1SC - Завершающий пакет после подсчета результата