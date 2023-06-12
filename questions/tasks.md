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
  const reversedStr = 
    str.split('').reverse().join('');
  return str.toLowerCase() === reversedStr.toLowerCase();
}
```

***

</details>

<details>
<summary>Проверка анаграммы</summary>

***

[Codewars - 7 kyu Anagram Detection](ttps://www.codewars.com/kata/529eef7a9194e0cbc1000255/javascript)

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




