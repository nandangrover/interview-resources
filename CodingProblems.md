# Coding Questions and Answers
### 1. Suppose a robber wants to steal from n number of houses in a night. The houses each have m amount of money. If the robber steal the money from adjacent houses then he will get caught. Since the houses are in a circle the first and last house are also adjacent.Return the maximum amout of money that the robber can steal from the houses. 
### For example:
### Input: [2,3,2] Output: 3
### Input: [1,2,3,4,5] Output: 6

```js
let indexOfMax = arr => {
  if (arr.length === 0) {
    return -1;
  }

  let max = arr[0];
  let maxIndex = 0;

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      maxIndex = i;
      max = arr[i];
    }
  }

  return maxIndex;
};

let isNeighbour = (indexes, index) => {
  return indexes.some(_index => Math.abs(index - _index) <= 1);
};

let maxMoney = input => {
  const newArr = [...input];

  let firstEncount = false;
  let lastEncount = false;

  let money = 0;
  const indexes = [];

  while (newArr.length) {
    const max = indexOfMax(newArr);
    const currIndex = input.indexOf(newArr[max]);

    if (newArr[max] === input[0]) firstEncount = true;
    if (newArr[max] === input[input.length - 1]) lastEncount = true;

    if (
      !isNeighbour(indexes, currIndex) &&
      !(firstEncount === true && lastEncount === true)
    ) {
      money += newArr[max];
      indexes.push(currIndex);
    }

    newArr.splice(max, 1);
  }
  return money;
};

```
