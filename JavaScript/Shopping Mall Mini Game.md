# 쇼핑몰 미니 게임 만들기
HTML, CSS, JavaScript를 활용해 만들기

## HTML
<details>
<summary>game.html</summary>
<div markdown="1">

```
<body>
    <!-- Logo -->
    <img src="imgs/logo.png" alt="Logo" class="logo" />

    <!-- Buttons -->
    <section class="buttons">
      <button class="btn">
        <img
          src="imgs/blue_t.png"
          alt="tshirt"
          class="imgBtn"
          data-key="type"
          data-value="tshirt"
        />
      </button>
      <button class="btn">
        <img
          src="imgs/blue_p.png"
          alt="pants"
          class="imgBtn"
          data-key="type"
          data-value="pants"
        />
      </button>
      <button class="btn">
        <img
          src="imgs/blue_s.png"
          alt="skirt"
          class="imgBtn"
          data-key="type"
          data-value="skirt"
        />
      </button>
      <button class="btn colorBtn blue" data-key="color" data-value="blue">
        Blue
      </button>
      <button class="btn colorBtn yellow" data-key="color" data-value="yellow">
        Yellow
      </button>
      <button class="btn colorBtn pink" data-key="color" data-value="pink">
        Pink
      </button>
    </section>

    <!-- Items -->
    <ul class="items">
        <!-- items는 game.js의 createHTMLString 함수에서 받는다. -->
    </ul>
  </body>
```
</div>
</details>

## CSS
<details>
<summary>game.css</summary>
<div markdown="1">

```
:root {
    /* color */
    --color-black: #3f454d;
    --color-white: #ffffff;
    --color-blue: #3b88c3;
    --color-yellow: #fbbe28;
    --color-pink: #fd7f84;
    --color-light-grey: #dfdfdf;
  
    /* size */
    --base-space: 8px;
    --size-button: 60px;
    --size-border: 4px;
    --size-thumbnail: 50px;
    --font-size: 18px;
  
    /* annimation */
    --animation: 300ms;
  }

/* body */
  body {
    height: 100vh;
    background-color: var(--color-black);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

/* logo */
  .logo {
    cursor: pointer;
    transition: transform var(--animation) ease;
  }

/* button */
  .btn {
    background-color: transparent;
    border: none;
    outline: none;
    cursor: pointer;
    transition: transform var(--animation) ease;
    margin-right: var(--base-space);
  }
  
  .btn:hover,
  .logo:hover {
    transform: scale(1.1);
  }
  
  .buttons {
    display: flex;
    align-items: center;
  }
  
  .imgBtn {
    width: var(--size-button);
    height: var(--size-button);
  }
  
  .colorBtn {
    font-size: var(--font-size);
    padding: calc(var(--base-space) * 2);
    border-radius: var(--size-border);
  }
  
  .blue {
    background-color: var(--color-blue);
  }
  
  .yellow {
    background-color: var(--color-yellow);
  }
  
  .pink {
    background-color: var(--color-pink);
  }

/* items */
  .items {
    width: 60%;
    height: 60%;
    list-style: none;
    padding-left: 0;
  }
  
  .item {
    background-color: var(--color-white);
    display: flex;
    align-items: center;
    padding: var(--base-space);
    margin-bottom: var(--base-space);
  }
  
  .item.invisible {
    background-color: var(--color-white);
    display: none;
  }
  
  .item__thumbnail {
    width: var(--size-thumbnail);
    height: var(--size-thumbnail);
  }
  
  .item__description {
    margin-left: var(--base-space);
    font-size: var(--font-size);
  }
  
```
</div>
</details>

## HTML & CSS 완성
![html css](https://user-images.githubusercontent.com/73509513/159412737-9ffc5700-234d-43e4-a291-ec7880ba73cc.PNG)

## json
<details>
<summary>data.json</summary>
<div markdown="1">

```
{
    "items": [
        {
            "type": "tshirt", 
            "gender": "male", 
            "size": "large", 
            "color": "blue", 
            "image": "imgs/blue_t.png"
        },
        {
            "type": "skirt", 
            "gender": "female", 
            "size": "small", 
            "color": "blue", 
            "image": "imgs/blue_s.png"
        },
        {
            "type": "pants", 
            "gender": "male", 
            "size": "large", 
            "color": "blue", 
            "image": "imgs/blue_p.png"
        },

        {
            "type": "tshirt", 
            "gender": "female", 
            "size": "small", 
            "color": "pink", 
            "image": "imgs/pink_t.png"
        },
        {
            "type": "skirt", 
            "gender": "female", 
            "size": "large", 
            "color": "pink", 
            "image": "imgs/pink_s.png"
        },
        {
            "type": "pants", 
            "gender": "male", 
            "size": "large", 
            "color": "pink", 
            "image": "imgs/pink_p.png"
        },

        {
            "type": "tshirt", 
            "gender": "female", 
            "size": "small", 
            "color": "yellow", 
            "image": "imgs/yellow_t.png"
        },
        {
            "type": "skirt", 
            "gender": "female", 
            "size": "small", 
            "color": "yellow", 
            "image": "imgs/yellow_s.png"
        },
        {
            "type": "pants", 
            "gender": "male", 
            "size": "large", 
            "color": "yellow", 
            "image": "imgs/yellow_p.png"
        }
    ]
}
```

</div>
</details>

## JavaScript
<details>
<summary>game.js</summary>
<div markdown="1">

```
'use strict'

// JSON 파일에서 items 목록 가져오기
function loadItems() {
    return fetch('data/data.json')
    .then(response => response.json())
    .then(json => json.items);
}
// fetch는 해당하는 경로나, url을 작성하면 간단하게 불러올 수 있다.
// data.json 호출

// items 목록 업데이트
function displayItems(items) {
    const container = document.querySelector('.items');
    container.innerHTML = items.map(item => createHTMLString(item)).join('');
}
// items를 html에 표시


// 가져온 items 목록을 html로 만들기
function createHTMLString(item) {
    return `
        <li class="item">
            <img src="${item.image}" alt="${item.type}" class="item__thumbnail" />
            <span class="item__description">${item.gender}, ${item.size}</span>
        </li>
    `;
}

// 이벤트 함수 만들기
function setEventListeners(items) {
    const logo = document.querySelector('.logo');
    const buttons = document.querySelector('.buttons');
    // document.querySelector: css 선택자를 가진 html 요소를 선택해서 가져온다.

    logo.addEventListener('click', () => displayItems(items));
    buttons.addEventListener('click', event => onButtonClick(event, items));
    // 클릭시 displayItems, onButtonClick 호출
}

// 버튼을 눌렀을 때 이벤트 발생
function onButtonClick(event, items) {
    const dataset = event.target.dataset;
    const key = dataset.key;
    const value = dataset.value;
    // html의 버튼들의 data-key, data-value 받음

    if(key == null || value == null) {
        return;
    }
    // key와 value의 값이 없으면 중단

    const filtered = items.filter(item => item[key] === value);
    displayItems(filtered);
    // item[key]와 value가 똑같은 것만 골라서 displayItems에 표시
}

// main
loadItems()
    .then(items => {
        console.log(items);               
        displayItems(items);
        setEventListeners(items)
    })
.catch(console.log)
// loadItems()는 data.json에 있는 데이터를 읽어와서 items을 전달, promise로 리턴하도록 작성
// items를 동적으로 받아와서 promise로 전달, 성공적으로 값을 전달해주면 html에 items를 보여주고,
// setEventListeners를 이용해서 blue, yellow, pink 버튼을 눌렀을때 필터링 된다.

```

</div>
</details>

## JavaScript 완성

### T-Shirt
![1](https://user-images.githubusercontent.com/73509513/160037759-ca111bf1-6dac-4d14-9f15-d600bd6fc5be.PNG)

### Pants
![2](https://user-images.githubusercontent.com/73509513/160037762-d8a7bf1e-ecb0-4b7a-bc6f-cfc71b77af92.PNG)

### Skirt
![3](https://user-images.githubusercontent.com/73509513/160037773-0301988c-e647-4b13-8c2e-0fecc40379df.PNG)

### Blue
![4](https://user-images.githubusercontent.com/73509513/160037777-a61e52a4-559b-4136-9680-87a49beb58e0.PNG)

### Yellow
![5](https://user-images.githubusercontent.com/73509513/160037784-2fa5485d-607d-4b5b-b717-f844367efcaa.PNG)

### Pink
![6](https://user-images.githubusercontent.com/73509513/160037792-57afb775-beb9-4dbc-9885-66051431890f.PNG)

## ★
`document.querySelector`
css 선택자를 가진 html 요소를 선택해서 가져온다.  

`return fetch()`
fetch는 해당하는 경로나, url을 작성하면 간단하게 불러올 수 있다.  
