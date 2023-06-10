## Практические задачи:

<details>
<summary>Проверка палиндрома</summary>

[Codewars - 8 kyu Is it a palindrome?](https://www.codewars.com/kata/57a1fd2ce298a731b20006a4)

Напишите функцию, которая принимает на вход строку и возвращает true, если она является палиндромом (читается одинаково слева направо и справа налево), и false в противном случае.  

```js
function isPalindrome(str) {
  const reversedStr = str.split('').reverse().join('');
  return str.toLowerCase() === reversedStr.toLowerCase();
}
```
</details>


<details>
<summary>Проверка самого кроткого слова</summary>

[Codewars - 7 kyu Shortest Word](https://www.codewars.com/kata/57cebe1dc6fdc20c57000ac9/javascript)

Просто, учитывая строку слов, вернуть длину кратчайшего слова (слов). Строка никогда не будет пустой, и вам не нужно учитывать разные типы данных.  

```js
function findShort(s){
  return s
    .split(' ')
    .sort((a,b) => a.length - b.length)
    .at(0)
    .length;
}
```
</details>


