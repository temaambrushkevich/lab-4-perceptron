# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил:
- Амбрушкевич Артем Антонович
- РИ-211002

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | # | 60 |
| Задание 2 | # | 20 |
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
- Выводы.

## Цель работы
В проекте Unity реализовать перцептрон.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR, также дать комментарии о корректности работы.
Ход работы:  
 * создал пустой 3D проект Unity.  
 * скачал скрипт ```Perceptron.cs``` с репозитория и добавил его в свой проект.  
   Содержание скрипта:
   ```c#
   using System.Collections;
   using System.Collections.Generic;
   using UnityEngine;

   [System.Serializable]
   public class TrainingSet
   {
      public double[] input;
      public double output;
   }

   public class Perceptron : MonoBehaviour {

     public TrainingSet[] ts;
     double[] weights = {0,0};
     double bias = 0;
     double totalError = 0;

     double DotProductBias(double[] v1, double[] v2) 
     {
        if (v1 == null || v2 == null)
         return -1;

        if (v1.Length != v2.Length)
         return -1;

        double d = 0;
        for (int x = 0; x < v1.Length; x++)
        {
         d += v1[x] * v2[x];
        }

        d += bias;

        return d;
     }

     double CalcOutput(int i)
     {
        double dp = DotProductBias(weights,ts[i].input);
        if(dp > 0) return(1);
        return (0);
     }

     void InitialiseWeights()
     {
        for(int i = 0; i < weights.Length; i++)
        {
         weights[i] = Random.Range(-1.0f,1.0f);
        }
        bias = Random.Range(-1.0f,1.0f);
     }

     void UpdateWeights(int j)
     {
        double error = ts[j].output - CalcOutput(j);
        totalError += Mathf.Abs((float)error);
        for(int i = 0; i < weights.Length; i++)
        {			
         weights[i] = weights[i] + error*ts[j].input[i]; 
        }
        bias += error;
     }

     double CalcOutput(double i1, double i2)
     {
        double[] inp = new double[] {i1, i2};
        double dp = DotProductBias(weights,inp);
        if(dp > 0) return(1);
        return (0);
     }

     void Train(int epochs)
     {
        InitialiseWeights();

        for(int e = 0; e < epochs; e++)
        {
         totalError = 0;
         for(int t = 0; t < ts.Length; t++)
         {
          UpdateWeights(t);
          Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
         }
         Debug.Log("TOTAL ERROR: " + totalError);
        }
     }

     void Start () {
        Train(5); // сколько тренировок
        Debug.Log("Test 0 0: " + CalcOutput(0,0));
        Debug.Log("Test 0 1: " + CalcOutput(0,1));
        Debug.Log("Test 1 0: " + CalcOutput(1,0));
        Debug.Log("Test 1 1: " + CalcOutput(1,1));		
     }
   }
   ```  
 * создал пустой объект с названием **Perceptron** и повешал на него скрипт **Perceptron.cs**  
 * далее научил "перцептрон" производить вычисление операции OR, для этого соответствующе OR заполнил таблицу истинности(рис. 1)
   - ![image](https://user-images.githubusercontent.com/97295011/201477233-2cee9d2e-1ce0-4019-96f8-30142cf78c37.png)  ***рис. 1***
   - в резултате запуска проекта, отработав 5 эпох обучения, перцептрон научился безошибочно производить операцию OR(рис. 2)
   - ![image](https://user-images.githubusercontent.com/97295011/201477668-5701e45b-7530-45f8-8c14-ee08e33685a2.png)  ***рис. 2***  
 * далее научил "перцептрон" производить вычисление операции AND, для этого соответствующе AND заполнил таблицу истинности(рис. 3)
   - ![image](https://user-images.githubusercontent.com/97295011/201477560-fb6fec69-fddc-4042-b27b-386161913cf1.png)  ***рис. 3*** 
   - в резултате запуска проекта, отработав 6 эпох обучения, перцептрон научился безошибочно производить операцию AND(рис. 4)
   - ![image](https://user-images.githubusercontent.com/97295011/201477600-7b329c3e-74ea-49c3-8ce3-616c9b4cb868.png)  ***рис. 4***   
 * далее научил "перцептрон" производить вычисление операции NAND, для этого соответствующе NAND заполнил таблицу истинности(рис. 5)  
   При операции NAND вначале происходит операция логического умножение, а затем операция логического отрицания. 
   - ![image](https://user-images.githubusercontent.com/97295011/201478154-7fc00012-e00b-42a7-aa6c-3f9138fcf641.png)  ***рис. 5***  
   - в резултате запуска проекта, отработав 8 эпох обучения, перцептрон научился безошибочно производить операцию NAND(рис. 6)  
   - ![image](https://user-images.githubusercontent.com/97295011/201478294-fb4d3546-29aa-4ccc-9a2c-88c01d39980a.png)  ***рис. 6*** 
 * с операцией XOR будет по-сложнее, научить "перцептрон" производить вычисление операции XOR(исключающее или) при текущих настройках, соответствующе XOR заполнив таблицу истинности(рис. 7) - не получится даже при ``` Train(1000000); ``` так как нейронная сеть с одним перцептроном может классифицировать только линейно-разделимые образы, поэтому для решения данной задачи следует добавить скрытый слой нейронов.
   - ![image](https://user-images.githubusercontent.com/97295011/201479117-76962d35-b375-4333-905d-89588480b6ed.png)  ***рис. 7*** 

  

   
 
## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
