# Array api

[Q1. 배열을 문자열로 변환하기](#q1-배열을-문자열로-변환하기)  
[Q2. 주어진 문자열을 배열로 변환하기](#q2-주어진-문자열을-배열로-변환하기)  
[Q3. 주어진 배열의 순서를 거꾸로 만들기](#q3-주어진-배열의-순서를-거꾸로-만들기-5-4-3-2-1)  
[Q4. 주어진 배열에서 첫번째와 두번째 요소를 제외한 나머지를 출력하기](#q4-주어진-배열에서-첫번째와-두번째-요소를-제외한-나머지를-출력하기)  
[Q5. 점수가 90점인 학생을 찾기](#q5-점수가-90점인-학생을-찾기)  
[Q6. 수업에 등록한 학생들만 골라내 배열로 만들기](#q6-수업에-등록한-학생들만-골라내-배열로-만들기)  
[Q7. 학생 배열에서 점수만 뽑아와서 점수만 들어있는 배열로 만들기](#q7-학생-배열에서-점수만-뽑아와서-점수만-들어있는-배열로-만들기)  
[Q8. 학생의 점수가 50보다 낮은 학생이 있는지 없는지 확인하기](#q8-학생의-점수가-50보다-낮은-학생이-있는지-없는지-확인하기)  
[Q9. 학생들의 평균점수를 구해오기](#q9-학생들의-평균점수를-구해오기)  
[Q10. 학생들의 모든 점수를 문자열로 변환해서 만들기](#q10-학생들의-모든-점수를-문자열로-변환해서-만들기)  
[Bonus! 학생들의 점수를 낮은 점수부터 정렬해서 문자열로 변환하기](#bonus-학생들의-점수를-낮은-점수부터-정렬해서-문자열로-변환하기)  

---


## Q1. 배열을 문자열로 변환하기
### Join: 배열에 각각 구분자를 넣어 string으로 리턴
```
{
  const fruits = ['apple', 'banana', 'orange'];
  const result = fruits.join(', and');
  console.log(result);
}
```
![1](https://user-images.githubusercontent.com/73509513/157154051-f57a51c8-3f71-4d22-866f-b6ae048bdcdd.PNG)

## Q2. 주어진 문자열을 배열로 변환하기
### split: string을 배열로 리턴하고, 리턴받을 배열의 사이즈를 지정할 수 있다.
```
{
  const fruits = '🍎, 🥝, 🍌, 🍒';
  const result = fruits.split (',', 2);
  console.log(result);
}
```
![2](https://user-images.githubusercontent.com/73509513/157154065-004c63fd-ed73-4714-a41d-d6b3d922c001.PNG)

## Q3. 주어진 배열의 순서를 거꾸로 만들기: [5, 4, 3, 2, 1]
### reverse: 배열안에 들어있는 요소를 거꾸로 만들어준다. 하지만 .reverse()를 호출한 array도 변경된다.
```
{
  const array = [1, 2, 3, 4, 5];
  const result = array.reverse();
  console.log(result);
}
```
![3](https://user-images.githubusercontent.com/73509513/157154082-c594f468-ccb3-4bdf-bfae-3232e6974023.PNG)

## Q4. 주어진 배열에서 첫번째와 두번째 요소를 제외한 나머지를 출력하기
### slice: 배열의 특정한 부분을 리턴, 주의할 점은 만약 0과 2까지라면 마지막 2는 배제되어 0과 1만 리턴한다.
**splice를 사용할 수 있지만 splice는 배열 전체에도 영향을 받아 배열의 일부분만 리턴하려면 slice를 쓰는것이 좋다.**
```
{
  const array = [1, 2, 3, 4, 5];
  const result = array.slice(2, 5);
  console.log(result);
  console.log(array);
}
```
![4](https://user-images.githubusercontent.com/73509513/157154099-07155838-0860-413b-a748-2ed0d0d7f00b.PNG)

---
### Q5~ 주어진 배열을 이용해 문제 풀기

```
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}

const students = [
  new Student('A', 29, true, 45),
  new Student('B', 28, false, 80),
  new Student('C', 30, true, 90),
  new Student('D', 40, false, 66),
  new Student('E', 18, true, 88),
];
```

## Q5. 점수가 90점인 학생을 찾기
### find: 첫번째로 찾아진 요소의 전달된 콜백함수가 true일때 리턴 찾지못하면 undefined 리턴  
**각각 학생의 점수를 받아왔을때 학생의 점수가 90점 일때 true를 리턴하고 아니면 false를 리턴**  
**fine는 첫번째로 true가 나오면 해당하는 그 배열의 요소를 리턴**  

```
{
  const result = students.find((student) => student.score === 90);
  console.log(result);
}
```
![5](https://user-images.githubusercontent.com/73509513/157154112-c4735d5e-78de-4179-8b4c-d92ed6271311.PNG)

## Q6. 수업에 등록한 학생들만 골라내 배열로 만들기
### filter: 원하는 배열만 받아올 수 있다.
```
{
  const result = students.filter((student) => student.enrolled);
  console.log(result);
}
```
![6](https://user-images.githubusercontent.com/73509513/157154120-99dcfecf-3d49-401b-ad03-f21fda59217e.PNG)

## Q7. 학생 배열에서 점수만 뽑아와서 점수만 들어있는 배열로 만들기
### 결과값: [45, 80, 90, 66, 88]

### map: 지정된 콜백함수를 호출하면서 각각의 요소를 함수를 거쳐 다시 새로운 값으로 맵핑해 변환
```
{
  const result = students.map((student) => student.score);
  console.log(result);
}
```
![7](https://user-images.githubusercontent.com/73509513/157154132-b062d9f6-0a23-4d62-8a49-709512a388b4.PNG)

## Q8. 학생의 점수가 50보다 낮은 학생이 있는지 없는지 확인하기
### some: 배열의 요소중에서 콜백함수가 리턴이 true가 되는 요소가 있는지 없는지 확인
### every: 모든 배열의 요소들이 조건을 충족해야 true가 리턴
```
{
  const result = students.some((student) => student.score < 50);
  console.log(result);

  const result2 = !students.every((student) => student.score >= 50);
  console.log(result2);
}
```
**!는 false라면 true로 true라면 false로 리턴**
![8](https://user-images.githubusercontent.com/73509513/157154142-d72342d6-04a7-4c4e-a0dd-1f46a349b9c0.PNG)

## Q9. 학생들의 평균점수를 구해오기
### reduce: reduce는 콜백함수와, initialValue를 전달하게 되는데 콜백함수는 배열의 모든 요소마다 호출된다.  
**콜백함수에서 리턴되는 값은 함께 누적된 결과값을 리턴**  
**배열의 값이 curr(배열의 요소)로 순차적으로 하나씩 전달되고, curr의 리턴된 값이 그 다음에 호출될때 prev(이전의 리턴된 값)로 연결되어진다.**  
**즉 reduce는 원하는 시작점부터 모든 배열을 돌면서 어떤값을 누적할 때 사용한다.**  

**recuceRight는 반대로 끝나는 지점부터 시작되어 누적된다.**  

**reduce의 사용법**  
``array.reduce(이전값(누적값), 현잿값, 인덱스, 요소) => { return }, 초깃값);``
```
{
  const result = students.reduce((prev, curr) => {
    console.log('----------');
    console.log(prev);
    console.log(curr);
    return prev + curr.score;
  }, 0);
  console.log(result);

  const result2 = students.reduce((prev, curr) => prev + curr.score, 0);
  console.log(result2 / students.length);
}
```
![9](https://user-images.githubusercontent.com/73509513/157154152-11bf9570-c717-42e6-b0a8-18224e3618df.PNG)

## Q10. 학생들의 모든 점수를 문자열로 변환해서 만들기
### 결과값: '45, 80, 90, 66, 88'
**map같은 api들은 배열 자체를 리턴하기 때문에 api를 섞어서 호출할 수 있다.**
**만약 50점 이상의 요소도 같이 출력하고 싶으면 .filter를 추가할 수 있다.**
```
{
  const result = students
  .map((student) => student.score)
  .filter((score) => score >= 50)
  .join();
  console.log(result);
}
```
![10](https://user-images.githubusercontent.com/73509513/157154162-1ee2758e-caaf-40c4-bd34-d5fe6f90b67f.PNG)

## Bonus! 학생들의 점수를 낮은 점수부터 정렬해서 문자열로 변환하기  
### 결과값: '45, 66, 80, 88, 90'  
### sort: 콜백함수는 a와 b 즉 이전값과 현재값이 전달이 되는데 만약 -값을 전달하게 되면 첫번째가 뒤에꺼보다 작다고 간주되 정렬된다.  
**(a, b)를 전달 받고 a - b b가 a보다 크다면 -이므로 정렬된다.**  
**만약 점수가 큰것부터 정렬하려면 반대로 (a, b) b - a로 작성하면 된다.**  
```
{
  const result = students.map((student) => student.score)
  .sort((a, b) => a - b)
  .join();
  console.log(result);
}
```
![11](https://user-images.githubusercontent.com/73509513/157154170-39638ffa-69ae-4217-beb8-18ff4fd40115.PNG)
