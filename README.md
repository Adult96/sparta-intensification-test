### [과제] 숙련주차 과제 답


## 1.추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음.
- dispatch함수 만들어준다음 addTodo함수로 객체 전달

```javascript
//Form.jsx
const dispatch = useDispatch();

…

const onSubmitHandler = event => {
    event.preventDefault();
    if (todo.title.trim() === '' || todo.body.trim() === '') return;

    dispatch(
      addTodo({
        id: id,
        title: todo.title,
        body: todo.body,
        isDone: false,
      })
    );
  };
```
## 2.추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.
- ADD_TODO케이스 구현부 return에 …state.todos 스프레드 연산자 사용해 todos아이템 가져온후 받아온 데이터를 추가

```javascript
//todos.js
switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload],
      };
```
## 3.삭제 기능이 동작하지 않음.
- 삭제 케이스 추가 하고 구현부 return에 todos아이템 가져온후 filter메서드 사용해서 해당하는 id가 아닌것들만 가져옴

```javascript
//todos.js
case DELETE_TODO:
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload),
      };
```
## 4.상세 페이지에 진입 하였을 때 데이터가 업데이트 되지 않음.
- useEffect훅 사용해서 id가 변경될때마다 getTodoByID함수를 이용해서 해당 데이터 store안에있는 todo에 저장

```javascript
//Detail.jsx
useEffect(() => {
    dispatch(getTodoByID(id));
  }, [dispatch, id]);
```
## 5.완료된 카드의 상세 페이지에 진입 하였을 때 올바른 데이터를 불러오지 못함.
- to={`/${index}`}를  to={`/${todo.id}`}로 변경 


```javascript
//List.jsx
<StLink to={`/${todo.id}`} key={todo.id}>
                  <div>상세보기</div>
                </StLink>
```

## 6.취소 버튼 클릭시 기능이 작동하지 않음.
- 함수 인자로 todo.id전달 해야하기 때문에 화살표 함수로 감싼후 전달


```javascript
//List.jsx
<StButton 
    borderColor='green'
    onClick={() => onToggleStatusTodo(todo.id)}
>
    {todo.isDone ? '취소!' : '완료!'}
</StButton>
```
