# Redux Toolkit Documentation

## 1. Giới thiệu

Redux Toolkit là bộ công cụ chính thức do đội ngũ Redux phát triển, giúp đơn giản hóa và tối ưu hóa việc quản lý trạng thái (state) trong các ứng dụng JavaScript, đặc biệt là với React. Bộ công cụ này cung cấp các API tiện dụng nhằm giảm bớt boilerplate code khi làm việc với Redux.

## 2. Tích hợp Redux Toolkit với React

Để tích hợp Redux Toolkit với React, ta cần bọc ứng dụng React bằng `Provider` để cung cấp store và state cho toàn bộ component.

Ví dụ:

```typescript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

## 3. Tạo Store với configureStore

Store là trung tâm lưu trữ toàn bộ state của ứng dụng. Với Redux Toolkit, ta sử dụng hàm `configureStore` để tạo store một cách nhanh chóng và dễ dàng.

Ví dụ:

```typescript
// src/app/store.ts
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../slices/counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer, // reducer của slice
  },
});

// RootState: Kiểu của toàn bộ state trong store (lấy từ hàm getState() của store)
export type RootState = ReturnType<typeof store.getState>;

// AppDispatch: Kiểu của dispatch trong store
export type AppDispatch = typeof store.dispatch;

export default store;
```

Giải thích:

-   `RootState`: Lấy kiểu trả về của `store.getState()`, giúp định nghĩa kiểu cho toàn bộ cấu trúc state của store.
-   `AppDispatch`: Xác định kiểu cho hàm `dispatch`, đảm bảo các action được dispatch theo đúng kiểu của store.

## 4. Tạo Custom Hooks: useAppDispatch và useAppSelector

Để hỗ trợ việc sử dụng `dispatch` và `selector` với kiểu dữ liệu rõ ràng, ta có thể tạo các custom hooks như sau:

Ví dụ:

```typescript
// src/app/hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

// Custom hook cho useDispatch có hỗ trợ type
export const useAppDispatch = () => useDispatch<AppDispatch>();

// Custom hook cho useSelector với kiểu RootState
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

## 5. Tạo Slice với createSlice

Slice là một phần của Redux dùng để quản lý state và xử lý các action liên quan đến một tính năng cụ thể. Sử dụng `createSlice` giúp định nghĩa state ban đầu, reducers và tự động tạo các action.

Ví dụ: Slice cho counter

```typescript
// src/slices/counterSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';
import { RootState } from '../app/store';

interface CounterState {
  count: number;
}

const initialState: CounterState = {
  count: 0,
};

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: state => {
      state.count += 1;
    },
    decrement: state => {
      state.count -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.count += action.payload;
    },
  },
});

// Export actions và selectors
export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export const selectCount = (state: RootState) => state.counter.count;

export default counterSlice.reducer;
```

## 6. Sử dụng Slice trong Component React

Các component React có thể truy cập state và gửi actions thông qua các hooks như `useSelector` và `useDispatch`.

Ví dụ:

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, selectCount } from '../slices/counterSlice';

const Counter = () => {
  const count = useSelector(selectCount);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>Tăng</button>
      <button onClick={() => dispatch(decrement())}>Giảm</button>
    </div>
  );
};

export default Counter;
```

## 7. Redux-thunk và Xử lý Actions Bất Đồng Bộ

Redux Toolkit tích hợp sẵn `redux-thunk`, cho phép xử lý các action bất đồng bộ, ví dụ như gọi API.

### 7.1. Tạo Async Thunk

```javascript
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId) => {
    const response = await fetch(`https://api.example.com/user/${userId}`);
    return response.json();
  }
);
```

### 7.2. Xử lý Async Thunk trong Slice với extraReducers

```typescript
const userSlice = createSlice({
  name: 'user',
  initialState: { name: '', age: 0, loading: false, error: null },
  reducers: {
    setUser: (state, action) => {
      state.name = action.payload.name;
      state.age = action.payload.age;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.name = action.payload.name;
        state.age = action.payload.age;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  },
});

export const { setUser } = userSlice.actions;
export default userSlice.reducer;
```

## 8. Sử dụng useSelector và useDispatch trong Component

Một ví dụ khác về việc sử dụng `useSelector` để truy cập state và `useDispatch` để gửi action:

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './counterSlice';

function Counter() {
  const count = useSelector(state => state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
    </div>
  );
}

export default Counter;
```

## 9. Sử dụng Selector với createSelector

Để tối ưu hóa việc truy cập và tính toán state, ta có thể sử dụng hàm `createSelector`.

Ví dụ:

```javascript
import { createSelector } from '@reduxjs/toolkit';

const selectUser = state => state.user;

export const selectUserName = createSelector(
  [selectUser],
  user => user.name // Lấy tên từ state user
);
```

## 10. Sử dụng RTK Query

RTK Query là công cụ mạnh mẽ để quản lý dữ liệu từ API. Nó tự động quản lý caching, fetching và updating dữ liệu.

Ví dụ:

```javascript
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const userApi = createApi({
  reducerPath: 'userApi',
  baseQuery: fetchBaseQuery({ baseUrl: '[https://api.example.com/](https://api.example.com/)' }),
  endpoints: (builder) => ({
    getUser: builder.query({
      query: (id) => `user/${id}`,
    }),
  }),
});

// Hook tự động được tạo ra để truy cập API
export const { useGetUserQuery } = userApi;
```

## 11. Sử dụng createEntityAdapter

`createEntityAdapter` giúp quản lý và chuẩn hóa dữ liệu dạng danh sách hoặc đối tượng, cung cấp các phương thức thêm, xóa, cập nhật dữ liệu một cách hiệu quả.

Ví dụ:

```javascript
import { createEntityAdapter } from '@reduxjs/toolkit';

const usersAdapter = createEntityAdapter();

// Khởi tạo state với adapter
const initialState = usersAdapter.getInitialState();

const usersSlice = createSlice({
  name: 'users',
  initialState,
  reducers: {
    addUser: usersAdapter.addOne,
    removeUser: usersAdapter.removeOne,
  },
});
```

## 12. Kết hợp Reducers với combineReducers

Khi có nhiều reducer riêng biệt, ta có thể kết hợp chúng thành một root reducer. Mặc dù `configureStore` thường tự xử lý việc này, nhưng `combineReducers` vẫn hữu ích trong một số trường hợp.

Ví dụ:

```javascript
import { combineReducers } from '@reduxjs/toolkit';
import counterReducer from '../slices/counterSlice';
import userReducer from '../slices/userSlice';

const rootReducer = combineReducers({
  counter: counterReducer,
  user: userReducer,
});
```

## 13. Cấu hình Middleware

Khi tạo store với `configureStore`, bạn có thể cấu hình middleware bổ sung nếu cần, bên cạnh middleware mặc định (bao gồm `redux-thunk`).

Ví dụ:

```javascript
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../slices/counterSlice';
import userReducer from '../slices/userSlice';
import customMiddleware from '../middleware/customMiddleware';

const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(customMiddleware),
  devTools: true, // Kích hoạt Redux DevTools
});

export default store;
