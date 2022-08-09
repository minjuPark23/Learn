```
가장 큰 차이점은 react는 UI 라이브러리, vue는 프레임워크이다.
```

## Vue

- 자바스크립트 프레임워크.
- 지정된 문법 방식으로만 개발 가능.
- 상태관리를 위해 vuex 사용.

#### 1. 코드 형태

- HTML, JS, CSS 코드 영역을 분리해서 작성
  - templete에는 HTML, script에는 JS, style에는 CSS를 작성
- 제공하는 방법을 그대로 사용해야 한다. -> 자유도는 낮으나 코드 작성이 용이
  
#### 2. 컴포넌트 분리

- 새로운 컴포넌트를 만들어 분리하기 위해서 파일을 만들고 templete, script, style을 모두 작성해야한다.
- props를 전달하는 과정에서 해당 컴포넌트를 사용하는 모든 파일에 작성해줘야 한다.

#### 3. 속도

- 속도 측면에서 미세하게 vue가 react에 비해 빠르다
  
  
## React

- UI 라이브러리로 라이브러리 일부분만 가져와서 사용할 수 있다. 
- 그러나 자체적으로 전역 상태관리, 라우팅, 빌드 시스템등을 지원하지 않아 별도의 라이브러리 Redux, Recoil, React-router-dom 등을 사용한다.
- 자바스크립트 문법을 응용해 자유롭게 개발

#### 1. 코드 형태

- JavaScript XML 형태로 코드를 작성하여 JavaScript 문법을 응용하기 때문에 JavaScript만으로 UI 로직과 DOM을 구현

#### 2. 컴포넌트 분리와 재사용

- 파일별로 컴포넌트를 분리할 수 있으며, 새로운 함수형 컴포넌트를 생산하고, props 형태로 전달하거나 다른 곳에서 재사용이 용이.

#### 3. TypeScript

- 자바스크립트의 정적 표현인 TypeScript 사용시 편리, 함수형 프로그래밍을 적극적으로 활용하기 쉽다.
- vue도 ts를 지원하지만, ts를 사용하기 위해 ts용 모듈을 사용하고 코드 변경이 필요하다.

```
사용성, 생산성, 가시성에는 Vue
트렌드, 범용성에는 React
```


## 참고
- [React와 Vue](https://codingapple.com/unit/why-use-vue-over-react/)
- [React vs Vue](https://nyol.tistory.com/148)
