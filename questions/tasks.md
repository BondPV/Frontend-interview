## Практические задачи:

<details>
<summary>Повторный Fetch запрос</summary>

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
<summary>Проверка пар скобок</summary>

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
<summary>Проверка самого кроткого слова</summary>

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

[Codewars - 8 kyu Find Maximum and Minimum Values of a List](https://www.codewars.com/kata/577a98a6ae28071780000989/train/javascript)

```js
const min = (list) => Math.min(...list);
const max = (list) => Math.max(...list);
```

***

</details>




