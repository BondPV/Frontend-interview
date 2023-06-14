## Практические задачи:

<details>
<summary>Повторный Fetch запрос</summary>

***

Напишите функцию, которая будет повторять запрос до тех пор, пока он не выполнится успешно или не будет достигнуто максимальное количество попыток.  

```js
async function makeRequest(url, maxRetries, delay) {
  try {
    const response = await fetch(url);
    if (response.ok) {
      return response.json();
    } else {
      throw new Error(Request failed with status ${response.status});
    }
  } catch (error) {
    if (maxRetries > 0) {
      await new Promise(resolve => setTimeout(resolve, delay));
      return makeRequest(url, maxRetries - 1, delay);
    } else {
      throw error;
    }
  }
}

// url - адрес, по которому нужно выполнить запрос;
// maxRetries - максимальное количество попыток выполнения запроса;
// delay - задержка между попытками выполнения запроса (в миллисекундах).

Вот пример использования этой функции:

const url = 'https://example.com/data';
const maxRetries = 3;
const delay = 1000;

try {
  const data = await makeRequest(url, maxRetries, delay);
  console.log(data);
} catch (error) {
  console.error(error);
}
```

***

</details>

<details>
<summary>Реализация debounce</summary>

***

Функция debounce используется для отложенного выполнения функции, чтобы избежать частых и ненужных вызовов. Она принимает функцию и время задержки в миллисекундах, и возвращает новую функцию, которая будет вызывать переданную функцию только после того, как прошло указанное время без вызовов.  

```js
function debounce(func, delay) {
  let isCooldown = false;

  return function() {
    if (isCooldown) return;

    func.apply(this, arguments);
    isCooldown = true;
    setTimeout(() => isCooldown = false, delay);
  };
}
```

***

</details>

<details>
<summary>Реализация throttle</summary>

***

throttle() — это функция, которая вызывает другую функцию, «пропуская» некоторые вызовы с определённой периодичностью.  

```js
function throttle(func, ms) {
  let isThrottled = false,
    savedArgs,
    savedThis;

  function wrapper() {
    if (isThrottled) { // (2)
      savedArgs = arguments;
      savedThis = this;
      return;
    }

    func.apply(this, arguments); // (1)

    isThrottled = true;

    setTimeout(function() {
      isThrottled = false; // (3)
      if (savedArgs) {
        wrapper.apply(savedThis, savedArgs);
        savedArgs = savedThis = null;
      }
    }, ms);
  }

  return wrapper;
}

// Вызов throttle(func, ms) возвращает wrapper.

// Во время первого вызова обёртка просто вызывает func и устанавливает состояние задержки (isThrottled = true).
// В этом состоянии все вызовы запоминаются в saveArgs / saveThis. Обратите внимание, что контекст и аргументы одинаково важны и должны быть запомнены. Они нам нужны для того, чтобы воспроизвести вызов позднее.
// … Затем по прошествии ms миллисекунд срабатывает setTimeout. Состояние задержки сбрасывается (isThrottled = false). И если мы проигнорировали вызовы, то «обёртка» выполняется с последними запомненными аргументами и контекстом.
// На третьем шаге выполняется не func, а wrapper, потому что нам нужно не только выполнить func, но и ещё раз установить состояние задержки и таймаут для его сброса.
```

***

</details>

<details>
<summary>Проверка пар скобок</summary>

***

[Codewars - 7 kyu Valid Parentheses](https://www.codewars.com/kata/6411b91a5e71b915d237332d/javascript)

Напишите функцию, которая принимает строку скобок и определяет, допустим ли порядок скобок. Функция должна возвращать true, если строка допустима, и false, если она недействительна.  

```js
function validParentheses(str) {
  const stack = [];
  const openBrackets = ['(', '[', '{'];
  const closeBrackets = [')', ']', '}'];

  for (let i = 0; i < str.length; i++) {
    const bracket = str[i];
    
    if (openBrackets.includes(bracket)) {
      stack.push(bracket);
    } else if (closeBrackets.includes(bracket)) {
      const lastOpenBracket = stack.pop();
      const correspondingOpenBracket = openBrackets[closeBrackets.indexOf(bracket)];
      
      if (lastOpenBracket !== correspondingOpenBracket) {
        return false;
      }
    }
  }

  return stack.length === 0;
}
```

***

</details>

<details>
<summary>Проверка палиндрома</summary>

***

[Codewars - 8 kyu Is it a palindrome?](https://www.codewars.com/kata/57a1fd2ce298a731b20006a4)

Напишите функцию, которая принимает на вход строку и возвращает true, если она является палиндромом (читается одинаково слева направо и справа налево), и false в противном случае.  

```js
function isPalindrome(str) {
  str = str.toLowerCase();

  return str === str.split('').reverse().join('');
}
```
[leetcode - 125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

```js
function isPalindrome(s) {
  s = s.toLowerCase().replace(/[^a-z0-9]/g, '');

  const centerStr = Math.floor(s.length / 2)
  
  for (let i = 0; i < centerStr; i++) {
    if (s[i] !== s[s.length - 1 - i]) {
      return false;
    }
  }
  
  return true;
}
```

***

</details>

<details>
<summary>Проверка анаграммы</summary>

***

[Codewars - 7 kyu Anagram Detection](https://www.codewars.com/kata/529eef7a9194e0cbc1000255/javascript)

Анаграмма - это слово или фраза, образованная путем перестановки букв другого слова или фразы. Для определения анаграммы необходимо проверить, содержат ли две строки одинаковые наборы символов.  

```js
function isAnagram(str1, str2) {
  // Удаляем пробелы и приводим строки к нижнему регистру
  str1 = str1.replace(/s/g, '').toLowerCase();
  str2 = str2.replace(/s/g, '').toLowerCase();

  // Сортируем символы в строках
  const str1Sorted = str1.split('').sort().join('');
  const str2Sorted = str2.split('').sort().join('');

  // Сравниваем отсортированные строки
  if (str1Sorted === str2Sorted) {
    return true;
  } else {
    return false;
  }
}
```

***

</details>

<details>
<summary>Проверка изограммы (слова, в которых нет повторяющихся букв)</summary>

***

[Codewars - 7 kyu Isograms](https://www.codewars.com/kata/54ba84be607a92aa900000f1/javascript)

Изограмма - это слово или фраза, в которой каждая буква встречается только один раз. Для определения изограммы необходимо проверить, содержит ли строка только уникальные символы.  

```js
function isIsogram(str) {
  str = str.toLowerCase();
  const charCount = {};

  for (let i = 0; i < str.length; i++) {
    const char = str[i];

    // Если символ уже встречался в строке, то это не изограмма
    if (charCount[char]) {
      return false;
    }

    // Добавляем символ в объект charCount
    charCount[char] = 1;
  }

  // Если все символы уникальны, то это изограмма
  return true;
}
```

```js
function isIsogram(str){
  const newStr = str.toUpperCase();
  const charsSet= new Set(newStr);
  
  return charsSet.size === newStr.length;
}
```

***

</details>

<details>
<summary>Проверка самого кроткого слова</summary>

***

[Codewars - 7 kyu Shortest Word](https://www.codewars.com/kata/57cebe1dc6fdc20c57000ac9/javascript)

Просто, учитывая строку слов, вернуть длину кратчайшего слова (слов). Строка никогда не будет пустой, и вам не нужно учитывать разные типы данных.  

```js
function findShort(str){
  return str
    .split(' ')
    .sort((a,b) => a.length - b.length)
    .at(0)
    .length;
}
```

***

</details>

<details>
<summary>Создать инициалы</summary>

***

[Codewars - 8 kyu Abbreviate a Two Word Name](https://www.codewars.com/kata/57eadb7ecd143f4c9c0000a3)

Напишите функцию для преобразования имени в инициалы. Имя состоит из двух слов с одним пробелом между ними.  

```js
function abbrevName(name){
  return initials = 
    name
      .split(' ')
      .map(e => e[0].toUpperCase())
      .join('.')
}
```

***

</details>

<details>
<summary>Суммирование всех чисел числа</summary>

***

[Codewars - 7 kyu Summing a number's digits](https://www.codewars.com/kata/52f3149496de55aded000410/javascript)

Напишите функцию, которая принимает число и возвращает сумму абсолютного значения каждой из десятичных цифр числа.  

```js
function sumDigits(number) {
  return Math.abs(number)
    .toString()
    .split('')
    .reduce((acc, num) => +acc + +num, 0);
}
```

***

</details>

<details>
<summary>Поиск минимального и максимального значений в массиве</summary>

***

[Codewars - 8 kyu Find Maximum and Minimum Values of a List](https://www.codewars.com/kata/577a98a6ae28071780000989/train/javascript)

```js
const min = (list) => Math.min(...list);
const max = (list) => Math.max(...list);
```

***

</details>

<details>
<summary>Найти наибольший и наименьший элемент в массиве, не используя Math.max и Math.min</summary>

***

```js
function findMinMax(arr) {
  let min = arr[0];
  let max = arr[0];

  for (let i = 1; i < arr.length; i++) {
    const item = arr[i]

    if (item < min) {
      min = item;
    } else if (item > max) {
      max = item;
    }
  }

  return { min, max };
}
```

***

</details>

<details>
<summary>Обработка данных в массиве определенным образом с решением O(n).</summary>

***

```js
// Необходимо обработать массив таким образом, чтобы распределить людей по группам городов

// Данные на вход
const people = [
  {
    name: 'Alex',
    city: 'Moscow',
  },
  {
    name: 'Ivan',
    city: 'Moscow',
  },
  {
    name: 'Joe',
    city: 'New York'
  },
  {
    name: 'Johan',
    city: 'Berlin'
  },
]

const groupByCity = (array) => {}

// Данные на выход
{
  'Moscow': [ 'Alex', 'Ivan' ],
  'New York': 'Joe',
  'Berlin': 'Johan'
}
```
```js
const groupByCity = (array) => {
  const result = {}

  for (const item of array) {
    const { city, name } = item

    if (!result[city]) {
      result[city] = name
    } else if(Array.isArray(result[city])) {
      result[city].push(name)
    } else {
      result[city] = [result[city], name]
    }
  }

  return result
}
```

***

</details>

<details>
<summary>Объединение интервалов в массиве</summary>

***

[leetcode - 56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)  
[Подробный разбор решения](https://www.youtube.com/watch?v=2Od3MV1-mpk)

```js
const array1 = [[1, 3], [2, 6], [8, 10], [15, 18]]; // [[1, 6], [8, 10], [15, 18]]
const array2 = [[1, 4], [4, 5]]; // [[1, 5]]
const array3 = [[11, 12], [2, 3], [5, 7], [1, 4], [8, 10], [6, 8]]; // [[1, 4], [5, 10], [11, 12]]

function merge(intervals) {
  if (intervals.length < 2) return intervals
  
  intervals.sort((a, b) => a[0] - b[0])
  
  let result = [intervals[0]]
  
  for (let interval of intervals) {
    let recent = result[result.length - 1];
    
    if (recent[1] >=  interval[0]) {
      recent[1] = Math.max(recent[1], interval[1])
    } else {
      result.push(interval)
    }
  }
  
  return result
}
```

***

</details>




