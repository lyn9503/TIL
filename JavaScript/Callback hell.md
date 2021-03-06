# Callback hell (콜백 지옥)

## 콜백 지옥 
```
class UserStorage {
    loginUser(id, password, onSuccess, onError) {
        setTimeout(()=> {
            if(
                (id === 'ellie' && password === 'dream') || 
                (id === 'coder' && password === 'academy')
            ) {
                onSuccess(id);
            } else {
                onError(new Error('not found'));
            }
        }, 2000);
    }

    getRoles (user, onSuccess, onError) {
        setTimeout(()=> {
            if (user === 'ellie') {
                onSuccess({name: 'ellie', role: 'admin'});
            } else {
                onError(new Error('no access'));
            }
        }, 1000);
    }
}
```
UserStorage는 총 두개의 api가 존재한다고 가정한다.  
loginUser는 id와 password를 받아오며, 로그인이 정상적으로 이루어지면 onSuccess를 호출하고, 로그인이 실패하면 onError이란 콜백함수를 호출해준다.  
getRoles는 사용자의 데이터를 받아서 사용자마다 가지고 있는 admin이나 geust등등을 서버에 다시 요청해서 받아온다. (원래는 백엔드에서 받아온다.)  
loginUser라는 함수를 호출하게 되면 2초뒤에 setTimeout이 실행되게 된다.  
setTimeout은 id가 ellie거나 coder, password가 dream이거나 academy면 onSuccess라는 콜백을 불러오며 id를 전달해준다.  
만약 포함되지 않으면 onError라는 object를 생성해서 not found를 전달해준다.  
getRoles은 1초의 지연을 가지고 사용자가 ellie이면 onSuccess를 호춣하면서 이름은 name이고 admin인 object를 전달해주며,  
ellie가 아니라면 onError를 호출하면서 object를 생성하고 no access를 전달해준다.

```
const userStorage = new UserStorage();
const id = prompt('enter your id'); // 아이디 입력
const password = prompt('enter your password'); // 비밀번호 입력
userStorage.loginUser(
    id,
    password,
    user => {
        userStorage.getRoles( // getRoles는 로그인이 성공하고 role이 맞으면 사용자의 정보를 받아온다.
            user, 
            userWithRole => {
                alert(`Hello ${userWithRole.name}, you have a ${userWithRole.role} role`);
            }, // 사용자의 데이터가 들어오면 id와 role이 어떤것인지 확인시켜 주는 알림을 띄운다.
            error => {
                console.log(error) // 맞지 않으면 onError의 not found를 출력한다.
            }
        );
    }, // 로그인이 성공했을때 불러오는 콜백함수
    error => {
        console.log(error)
    } // 로그인이 실패했을때 불러오는 콜백함수
);
```
## 로그인이 정상적으로 이루어진 경우  
![2](https://user-images.githubusercontent.com/73509513/158008567-a2573857-63a4-44d2-b4f6-6416df1e513b.PNG)
![3](https://user-images.githubusercontent.com/73509513/158008569-7ca590f0-814b-42fb-9683-8c2c527a35a8.PNG)
![4](https://user-images.githubusercontent.com/73509513/158008574-f2673287-4fe6-40db-83c9-2681a947c432.PNG)

## 로그인이 비정상적으로 이루어진 경우
![5](https://user-images.githubusercontent.com/73509513/158008589-574c40b4-46cb-4e86-bec3-69a521651507.PNG)

## 콜백 지옥으로 작성한 경우 문제점
콜백을 이용해서 콜백함수 안에서 무언가 다른걸 호출하고 또 안에서 콜백함수를 호출... 이러한 것이 콜백 지옥이다.  
콜백 지옥의 문제점은 가독성이 떨어진다. 어디서 어떤식으로 연결되어 있는지 한눈에 가늠하기 어렵고, 비즈니스 로직을 이해하는 것이 어려우며,  
로그인 한 다음에 그 데이터를 이용해서 사용자의 역할을 받아오는 구나 라는 것을 알아보기 힘들다.  
또한 디버깅을 해야하는 경우에도 이해하기 힘들다. 만약에 위 코드보다 더 길어질 경우 무슨 문제가 생기면 에러의 콜백이 너무 많아 어디서 오류가 생기는지 이해하기 매우 힘들다.  

## 콜백 지옥 예제를 promise를 이용해 변경

```
class UserStorage {
    loginUser(id, password) {
        return new Promise((resolve, reject) => {
            setTimeout(()=> {
                if(
                    (id === 'ellie' && password === 'dream') || 
                    (id === 'coder' && password === 'academy')
                ) {
                    resolve(id);
                } else {
                    reject(new Error('not found'));
                }
            }, 2000);
        });
    }
    getRoles (user) {
        return new Promise((resolve, reject) => {
            setTimeout(()=> {
                if (user === 'ellie') {
                    resolve({name: 'ellie', role: 'admin'});
                } else {
                    reject(new Error('no access'));
                }
            }, 1000);
        });
    }
}
```
onSuccess, onError는 promise를 이용하므로 삭제  
promise 콜백 함수를 만들고 onSuccess, onError를 resolve, reject로 변경  

```
const userStorage = new UserStorage();
const id = prompt('enter your id');
const password = prompt('enter your password');
userStorage
    .loginUser(id, password)
    .then(userStorage.getRoles)
    .then(alert(`Hello ${userWithRole.name}, you have a ${userWithRole.role} role`))
    .catch(console.log);
```
userStorage에서 로그인을 했을 때 로그인이 성공한다면 userStorage의 getRoles를 호출하게 되고,  
모든 기능이 성공적으로 작동했을 때 최종적으로 받아오는 user를 이용해서 alert창을 띄우게 된다.  
만약 수행할 시 문제가 발생한다면 error 메세지가 console창에 나타나게 된다.  

이전 콜백 지옥 예제를 promise를 이용해 변경하게 되면 콜백 함수를 연속으로 사용할 필요 없이,  
then과 catch를 사용해 성공, 실패를 더욱 명확하게 보여주어 코드가 간결해졌다.

### promise로 변경한 콜백 지옥 예제에 async를 이용하기
```
class UserStorage {
    loginUser(id, password) {
        return new Promise((resolve, reject) => {
            setTimeout(()=> {
                if(
                    (id === 'ellie' && password === 'dream') || 
                    (id === 'coder' && password === 'academy')
                ) {
                    resolve(id);
                } else {
                    reject(new Error('not found'));
                }
            }, 2000);
        });
    }
    getRoles (user) {
        return new Promise((resolve, reject) => {
            setTimeout(()=> {
                if (user === 'ellie') {
                    resolve({name: 'ellie', role: 'admin'});
                } else {
                    reject(new Error('no access'));
                }
            }, 1000);
        });
    }
}

const userStorage = new UserStorage();
const id = prompt('enter your id');
const password = prompt('enter your password');

async function loginUser() {
    try{
        const userId = await userStorage.loginUser(id, password);
        const user = await userStorage.getRoles(userId);
        alert(`Hello ${user.name}, you have a ${user.role} role`);
    } catch (error) {
        console.log(error);
    }
}

loginUser();
```

이전의 promise는 then chaining을 사용했지만 코드를 보기에 조금 난잡해진다.  
이전코드를 삭제하고 async를 사용해 loginUser란 함수를 생성했으며, try, catch 예외 처리 문법을 사용해 성공, 실패 부분으로 나누었다.  
try에는 사용자의 id와 password, 역할이 모두 맞으면 알림을 띄워 사용자의 id와 역할을 알려주게 했으며,  
catch에는 id나 password가 맞지 않는 경우 error를 console에 띄우도록 했다.  
