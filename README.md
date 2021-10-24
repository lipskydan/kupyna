# Геш-функція Купина

<table>
    <tr>
        <td>Довжина геш-коду (n) </td>
        <td>Розмір внутрішнього стану (l)</td>
        <td>Кількість раундів (t)</td>
        <td>Рядків у матриці стану (с)</td>
    </tr>
    <tr>
        <td>8 <= n <= 256 </td>
        <td>512</td>
        <td>10</td>
        <td>8</td>
    </tr>
<tr>
        <td>256 <= n <= 512 </td>
        <td>1024</td>
        <td>14</td>
        <td>16</td>
    </tr>
</table>

## Алгоритм

### Попередня обробка

Спочатку повідомлення доповнюється до довжини, кратної розміру блока. Для цього до повідомлення додається 1 біт, 
після цього k нульових бітів, де <br>
k=(-N-97) mod l <br>
і 96 біт, які містять довжину повідомлення в бітах. <br>
Таким чином, максимальна довжина повідомлення становить 2^{96}-1 біт.

Далі повідомлення розбивається на t блоків по l біт у кожному. <br>

Для варіантів функції, які повертають до 256 біт включно, l = 512.<br>
Для варіантів, які повертають значення, довші 256 біт, l = 1024.

Далі, будується геш-функція, з використанням наступного ітеративного алгоритму.

<p align="center">
  <img src="/img/1.png" width="350">
</p>

де

v =1,2,...,k

IV=1<<<510, якщо l = 512, або 1<<1023, якщо l = 1024

R_{l,n}(X) — функція, яка повертає n найбільш значущих бітів блока розміром l

Кількість ітерацій для варіантів функції, які повертають до 256 біт включно — 10 <br> 
Кількість ітерацій для варіантів функції, які повертають значення, довші 256 біт — 14

### Перестановки T⊕ і T+

<p align="center">
  <img src="/img/2.png" width="350">
</p>

* Ці перетворення керують станом, представленим матрицею G, яка містить у кожній комірці 1 байт інформації. 
Матриця має розмір 8 на 8 при l=512 або 8 на 16 при l=1024

* Функція &kappa;<sup>(l)</sup><sub>v</sub> додає по модулю 2 вектор

* Функція &eta;<sup>(l)</sup><sub>v</sub> додає по модулю 2<sup>64</sup> вектор

* Функція &pi;<sup>'</sup> підміняє елементи матриці стану g<sub>i,j</sub> підстановкою з одного з чотирьох S-блоків (номер S-блока визначається як i mod 4 

* Функція &tau;<sup>l</sup> виконує циклічний зсув вправо елементів матриці стану. Рядки з номерами i=0,1,2,3,4,5,6 зсуваються на i елементів, а рядок 7 зсувається на 7 елементів при l=512 або на 11 елементів при l=1024

* &psi; - лінійне перетворення, множення вектора й матриці над скінченним полем. 
* * Для виконання функції &psi; кожен елемент матриці стану представляється як елемент скінченного поля GF(2^{8}), 
* * сформованого незвідним поліномом x^{8}+x^{4}+x^{3}+x^{2}+1. 
* * Кожен елемент матриці стану u_{i,j} обчислюється за формулою:<br> u_{i,j} = (v >>> i) ⊕ G_j, де <br>
 v — вектор (0x01, 0x01, 0x05, 0x01, 0x08, 0x06, 0x07, 0x04), а j — номер стовпця матриці стану G
