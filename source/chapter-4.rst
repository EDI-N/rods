Розділ 3. Інструкції для розробників щодо роботи з відкритими даними про регуляторні акти
############################################################


3.1. Принципи дизайну структури набору даних
**************************************************
Для розробки рекомендацій для розпорядників використані наступні принципи:
		
		.. hlist::
			:columns: 1

			- простота використання таблиць розпорядниками є важливішою за нормалізацію структури моделі даних;
			- виконання вимог рекомендацій має дати розробникам можливість легко приводити дані до нормалізованої структури;
			- нормалізована структура бази даних може бути реалізована у програмному забезпеченні, веб-сервісах, де графічний інтерфейс полегшуватиме введення та роботу з даними для розпорядників;
			- значення та метадані полів таблиці відповідають вимогам основних словників (`Dublin Core Metadata Initiative <http://dublincore.org/>`_, `ISA2 <https://ec.europa.eu/isa2/solutions/core-vocabularies_en>`_ та ін.) для забезпечення семантичної інтероперабельності й можливості створення RDF-серіалізацій;
			- рекомендації мають забезпечити можливість експорту даних розпорядниками у відкритих машиночитаних форматах;
			- для того, щоб виконати вимоги рекомендацій, розпорядникам достатньо мати навички впевнених користувачів електронних таблиць (Microsoft Excel, LibreOffice Calc, Google Таблиці);
			- структура набору має відповідати рекомендаціям W3C `Metadata Vocabulary for Tabular Data <https://www.w3.org/TR/tabular-metadata/>`_. Розпорядники мають можливість оприлюднювати JSON або CSV файл, що відповідає вимогам стандарту.


3.2. Валідація даних у таблиці listOfRegulatoryActs
**************************************************
Валідація даних таблиці listOfRegulatoryActs визначається наступними умовами та способами (див. Таблиця 3.2.).

		.. csv-table:: Таблиця 3.2. Валідація даних таблиці listOfRegulatoryActs
			:header-rows: 1

			Назва колонки,Умова валідації,"Спосіб валідації (регулярний вираз, словник)"
			identifier,Комірка має бути заповненою.,^.+$
			regulatoryAgency PrefLabel,Комірка має бути заповненою.,^.+$
			regulatoryAgency Identifier,Комірка має містити номер юридичної особи у ЄДР (8 цифр) або null.,^(?:\\d{8}|null)$
			title,Комірка має бути заповненою.,^.+$
			valid,Значення відповідає формату ISO 8601 (рррр-мм-дд).,^\\d{4}-(?:[0][1-9]|1[0-2])-(?:0[1-9]|[1-2]\\d|3[01])$
			accessURL,Посилання має розпочинатися з http:// або https:// (стандарт RFC 3986).,^https?:\\/\\/.+$
			bibliographicCitation,Комірка має бути заповненою.,^.+$
			legalActTitle,Комірка має бути заповненою.,^.+$
			legalActCreated,Значення відповідає формату ISO 8601 (рррр-мм-дд).,^\\d{4}-(?:[0][1-9]|1[0-2])-(?:0[1-9]|[1-2]\\d|3[01])$
			legalActІdentifier,Комірка має бути заповненою.,^.+$
			basicEvaluation Date,Значення відповідає формату ISO 8601 (рррр-мм-дд).,^\\d{4}-(?:[0][1-9]|1[0-2])-(?:0[1-9]|[1-2]\\d|3[01])$
			basicEvaluation AccessURL,Посилання має розпочинатися з “http://” або “https://” (стандарт RFC 3986) або має бути вказано “не застосовується”.,^(?:https?:\\/\\/.+|не застосовується)$
			basicEvaluation BibliographicCitation,Комірка має бути заповненою.,^.+$
			repeatedEvaluation Date,Значення відповідає формату ISO 8601 (рррр-мм-дд).,^\\d{4}-(?:[0][1-9]|1[0-2])-(?:0[1-9]|[1-2]\\d|3[01])$
			repeatedEvaluation AccessURL,Посилання має розпочинатися з “http://” або “https://” (стандарт RFC 3986) або має бути вказано “не застосовується”.,^(?:https?:\\/\\/.+|не застосовується)$
			repeatedEvaluation BibliographicCitation,Комірка має бути заповненою.,^.+$
			periodicEvaluation Date,Значення відповідає формату ISO 8601 (рррр-мм-дд).,^\\d{4}-(?:[0][1-9]|1[0-2])-(?:0[1-9]|[1-2]\\d|3[01])$
			periodicEvaluation AccessURL,Посилання має розпочинатися з “http://” або “https://” (стандарт RFC 3986) або має бути вказано “не застосовується”.,^(?:https?:\\/\\/.+|не застосовується)$
			periodicEvaluation BibliographicCitation,Комірка має бути заповненою.,^.+$



3.3. Модель даних та RDF-серіалізації набору
**************************************************

Модель включає п’ять класів: Регуляторний акт (RegulatoryAct), Відстеження результативності регуляторного акта (Evaluation), Звіт про відстеження результативності (Report), Регуляторний орган (RegulatoryAgency), Нормативно-правовий акт, яким було ухвалено регуляторний акт (LegalAct). UML-модель представлена на Рисунку 3.3.

	.. centered:: *Рисунок 3.3. UML-модель даних про регуляторні акти*

	.. image:: assets/uml-model.svg
		:alt: Рисунок 3.3. UML-модель даних про регуляторні акти
		:width: 550 px
		:align: center


Для синтаксичної прив’язки використані словники `Dublin Core Terms <http://dublincore.org/>`_, `FOAF <http://xmlns.com/foaf/spec/>`_, `Schema <https://schema.org/>`_, `The Organization Ontology <https://www.w3.org/TR/vocab-org/>`_, `SKOS <https://www.w3.org/TR/swbp-skos-core-spec/>`_, `RDF Schema <https://www.w3.org/TR/rdf-schema/>`_ (див. Таблицю 3.2а.).


		.. csv-table:: Таблиця 3.3а. Використання основних словників
			:header-rows: 1

			Назва словника,Префікс,Простір імен
			Dublin Core Terms,dct,http://purl.org/dc/terms/
			FOAF,foaf,http://xmlns.com/foaf/0.1/
			Schema,schema,http://schema.org/
			The Organization Ontology,org,http://www.w3.org/ns/org#
			SKOS,skos,http://www.w3.org/2004/02/skos/core#
			RDF Schema,rdfs,http://www.w3.org/2000/01/rdf-schema#


		.. csv-table:: Таблиця 3.3б. Прив’язка моделі до існуючого синтаксису словників
			:header-rows: 1

			Термін,Тип,Прив’язка синтаксису
			Регуляторний акт (RegulatoryAct),Клас,foaf:document
			Ідентифікатор,Властивість,dct:identifier
			Назва,Властивість,dct:title
			Дата набрання чинності,Властивість,dct:valid
			"Регуляторний орган, який ухвалив регуляторний акт",Асоціація,dct:creator
			"Нормативно-правовий акт, яким було ухвалено регуляторний акт",Асоціація,rdfs:isDefinedBy
			Відстеження результативності регуляторного акта (Evaluation),Клас,schema:Event
			Ідентифікатор,Властивість,dct:identifier
			Тип відстеження,Властивість,dct:type
			Дата початку відстеження,Властивість,schema:startDate
			Дата завершення відстеження,Властивість,schema:endDate
			Регуляторний акт щодо якого проводиться відстеження,Асоціація,dct:isRequiredBy
			Звіт про відстеження результативності (Report),Клас,foaf:document
			Ідентифікатор,Властивість,dct:identifier
			Дата затвердження,Властивість,dct:dateAccepted
			URL для доступу,Властивість,dct:accessURL
			Бібліографічне посилання,Властивість,dct:bibliographicCitation
			"Обстеження, за результатами якого затверджено звіт",Асоціація,foaf:primaryTopic
			Регуляторний орган (RegulatoryAgency),Клас,org:Organization
			Ідентифікатор,Властивість,org:identifier
			Назва,Властивість,skos:prefLabel
			"Нормативно-правовий акт, яким було ухвалено регуляторний акт (LegalAct)",Клас,foaf:document
			Ідентифікатор,Властивість,dct:identifier
			Назва,Властивість,dct:title
			Дата ухвалення,Властивість,dct:created

Завантажити RDF-схему можна за посиланням - :download:`rdf_schema_0.9.ttl <assets/rdf_schema_0.9.ttl>`.

.. підхід legislation.gov.uk: https://www.legislation.gov.uk/developer/formats/rdf