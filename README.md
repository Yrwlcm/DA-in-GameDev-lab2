# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Холстинин Егор Алексеевич
- РИ220943
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1. Выберите одну из компьютерных игр, приведите скриншот её геймплея и краткое описание концепта игры. Выберите одну из игровых переменных в игре (ресурсы, внутри игровая валюта, здоровье персонажей и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.

### Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач
Ход работы:
- В качестве игры для описания я выбрал Dota 2. Dota 2 — многопользовательская командная компьютерная игра в жанре MOBA. Игра изображает сражение на карте особого вида; в каждом матче участвуют две команды по пять игроков, управляющих разными «героями» — персонажами с различными наборами способностей и характеристиками. Для победы в матче команда должна уничтожить особый объект — «крепость», принадлежащий вражеской стороне, и защитить от уничтожения собственную «крепость».

![image](https://github.com/Yrwlcm/DA-in-GameDev-lab2/assets/99079920/36e61d1d-cef9-4b94-970d-bbe953f70422)

- В качестве валюты я выбрал золото. 
Золото — валюта, используемая для покупки предметов или мгновенного возрождения героя. Золото можно заработать путём убийства героев, крипов или при уничтожении построек. Количество золота может варьироваться от 0 до 99999, но чаще всего это значение не больше 10000, так как нет предметов, которые стоят больше 8000.

![image](https://github.com/Yrwlcm/DA-in-GameDev-lab2/assets/99079920/acadfe1e-8ba9-4feb-a3a2-25ab54fae39b)


## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в выбранной игре (в качестве таких переменных может выступать игровая валюта, ресурсы, здоровье и т.д.). Средствами google-sheets визуализируйте данные в google-таблице (постройте график, диаграмму и пр.) для наглядного представления выбранной игровой величины.

- Создать гугл таблицу, настроить api для неё.
- Заполнить её с помощью python кода
```py
import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience-400712-fb36790122ff.json')
sh = gc.open("UnityDataScience")
price = np.random.randint(100, 3000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((price[i-1]-price[i-2])/price[i-2])*100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)
```
- На основе полученных данных создать таблицу
![image](https://github.com/Yrwlcm/DA-in-GameDev-lab2/assets/99079920/b6ca3a0b-03bb-4018-ae29-fb1c4b7e18a4)

- Ссылка на таблицу: [https://docs.google.com/spreadsheets/d/19U0ApQEKWFRjuo-_uTfJE_fvKeSGlTWjF_Sjwmtxrfo/edit#gid=0](https://docs.google.com/spreadsheets/d/19U0ApQEKWFRjuo-_uTfJE_fvKeSGlTWjF_Sjwmtxrfo/edit?usp=sharing)

## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.

- Создать новый 3д проект на Unity и добавить в него нужные пакеты для обработки json-файлов и воспроизведения звука.
- Создать на сцене пустой объект и подключить к нему скрипт со следующим кодом.
- Из-за специфики игры (больше золота == лучше) я поменял воспроизведение звуков в зависимости от инфляции. А именно инфляция >= 100 воспроизводится хорошее аудио, инфляция <= 10 воспроизводится плохое аудио.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
	public AudioClip goodSpeak;
	public AudioClip normalSpeak;
	public AudioClip badSpeak;
	private AudioSource selectAudio;
	private Dictionary<string, float> dataSet = new Dictionary<string, float>();
	private bool statusStart = false;
	private int i = 1;

	// Start is called before the first frame update
	void Start()
	{
		StartCoroutine(GoogleSheets());
	}

	// Update is called once per frame
	void Update()
	{
		if (dataSet.Count == 0) return;

		if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
		{
			StartCoroutine(PlaySelectAudioBad());
			Debug.Log(dataSet["Mon_" + i.ToString()]);
		}

		if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
		{
			StartCoroutine(PlaySelectAudioNormal());
			Debug.Log(dataSet["Mon_" + i.ToString()]);
		}

		if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
		{
			StartCoroutine(PlaySelectAudioGood());
			Debug.Log(dataSet["Mon_" + i.ToString()]);
		}
	}

	IEnumerator GoogleSheets()
	{
		UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/19U0ApQEKWFRjuo-_uTfJE_fvKeSGlTWjF_Sjwmtxrfo/values/Лист1?key=AIzaSyASnu9QXtSW-cHLv__q0mYZNbbhJUFUgdM");
		yield return curentResp.SendWebRequest();
		string rawResp = curentResp.downloadHandler.text;
		var rawJson = JSON.Parse(rawResp);
		foreach (var itemRawJson in rawJson["values"])
		{
			var parseJson = JSON.Parse(itemRawJson.ToString());
			var selectRow = parseJson[0].AsStringList;
			dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
		}
	}

	IEnumerator PlaySelectAudioGood()
	{
		statusStart = true;
		selectAudio = GetComponent<AudioSource>();
		selectAudio.clip = goodSpeak;
		selectAudio.Play();
		yield return new WaitForSeconds(5);
		statusStart = false;
		i++;
	}
	IEnumerator PlaySelectAudioNormal()
	{
		statusStart = true;
		selectAudio = GetComponent<AudioSource>();
		selectAudio.clip = normalSpeak;
		selectAudio.Play();
		yield return new WaitForSeconds(3);
		statusStart = false;
		i++;
	}
	IEnumerator PlaySelectAudioBad()
	{
		statusStart = true;
		selectAudio = GetComponent<AudioSource>();
		selectAudio.clip = badSpeak;
		selectAudio.Play();
		yield return new WaitForSeconds(4);
		statusStart = false;
		i++;
	}
}

```


```

## Выводы

В ходе данной лабораторной работы я научился работать с api google sheets, заполнять гугл-таблицу с помощью кода python. А также работать с данными из гугл-таблиц напрямую из unity.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
