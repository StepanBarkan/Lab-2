# Виртуальная и дополненная реальность
Отчет по лабораторной работе #2 выполнил(а):
- Баркан Степан Николаевич
- РИ300022
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

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
изучить работу пакета Unity XR, настройку VR оборудования, запуск VR проекта через настроенное оборудование.

## Задание 1
### В разделе «ход работы» пошагово выполнить каждый пункт с описанием и примера реализации задач по теме видео самостоятельной работы.
Ход работы:
– Создайте новый Unity проект из шаблона 3D-Core;
– Откройте вкладку Edit -> Project settings;
– Установите XR Plugin Management;
– Настройте XR Plugin Management на работу через SDK OpenXR;
– Настройте режим рендера VR на каждый глаз;
– Добавить поддержку контроллеров вашего оборудования;
– Через вкладку Windows -> Pacage Manager добавьте и установите пакет
com.unity.xr.interaction.toolkit;
– Импортируйте Starter Assets из установленного пакета;
– Настройте Input system на основе импортированного Starter Assets;
– Скачайте и установите Steam и Steam VR;
– Настройте и подключите к PC ваше VR оборудование;
– Вернитесь в Unity и настройте запуск проекта через SteamVR;
– Добавьте объект Plane;
– Добавьте на сцену объект XR-Orig (Action Base);
– На объект XR Interaction Manager создайте компонент Input Action
Manager;
– Добавьте в Input Action Manager настроенный Input System;
– Запустите проект и убедитесь, что он воспроизводится на VR
оборудовании.

Cкрипт
```c#
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

[RequireComponent(typeof(ARRaycastManager))]
public class ObjectonPlane : MonoBehaviour
{
    [SerializeField]
    [Tooltip("Поля для добавления Plane Prefab")]
    GameObject mPlacedPrefab;
    UnityEvent placementUpdate;

    [SerializeField]
    GameObject selectObjectPrefab;

    public GameObject plasedPrefab
    {
        get
        {
            return mPlacedPrefab;
        }

        set
        {
            mPlacedPrefab = value;
        }
    }

    public GameObject spawnedObject
    {
        get;
        private set;
    }

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();

        if (placementUpdate == null)
        {
            placementUpdate = new UnityEvent();
            placementUpdate.AddListener(DisableVisual);
        }

    }
    bool getTouchPosition(out Vector2 touchPosition)
    {
        if (Input.touchCount > 0)
        {
            touchPosition = Input.GetTouch(0).position;
            return true;
        }

        touchPosition = default;
        return false;
    }

    private void Update()
    {
        if (!getTouchPosition(out Vector2 touchPosition))
        {
            return;
        }

        if (raycastManager.Raycast(touchPosition, selectHits, TrackableType.PlaneWithinPolygon))
        {
            var hitPose = selectHits[0].pose;
            if (spawnedObject == null)
            {
                spawnedObject = Instantiate(mPlacedPrefab, hitPose.position, hitPose.rotation);
            }
            else
            {
                spawnedObject.transform.position = hitPose.position;
            }
            placementUpdate.Invoke();

        }
    }

    public void DisableVisual()
    {
        selectObjectPrefab.SetActive(false);
    }

    static List<ARRaycastHit> selectHits = new List<ARRaycastHit>();
    ARRaycastManager raycastManager;
}
```

## Задание 2
### Продемонстрируйте на сцене в Unity следующее:
– Что значит X в аббревиатуре XR?
– Какие SDK поддерживает XR Plugin Management по Default?
Ответ:
1)Из родительской точки 0 берется берутся координаты дочернего объекта. Если перетащить объект и сделать его родительским, то его координаты сменят числовое значение относительно всей сцены, но объект положения не поменяет.

2)Параметром Use Gravity отвечает за гравитацию объекта. IsKinematic при включении отвечает за то, что на объект больше не действует никакая физиак, так например, если его толкнули, или заставили вращаться, то действие сли просто прекратится Так, поскольку надо было выполнить три пункта, то я ещё настраивал вес объекта. Как оказалось, этот параметр влияет на импульс объекта при соприкосновенни. Вес объекта, этот параметр влияет на импульс объекта при соприкосновенни. Так, если вес объекта 0, то после удара о другой объект, на этот друго объект не будет действовать никакая сила

## Выводы
В этой работе я изучил пакетUnity XR, настройку VR оборудования, запуск VR проекта через настроенное оборудование
