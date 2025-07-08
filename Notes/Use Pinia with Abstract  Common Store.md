---
category: note
type: tip
tags:
  - vue
  - pinia
  - store
  - ts
description: 創造一個共用的抽象 store 減少樣版 code
resource_1: https://www.youtube.com/watch?v=OTW8a2eRjRU
---

```ts
// useCommonStore

import { Ref } from "nuxt/dist/app/compat/capi";

// 型別建構，一開始先寫好抽象的State型別
export type MakeInitStateFn<State extends any> = (
  options?: Partial<State>
) => State;

type NewState<State> = Partial<State> | ((state: State) => Partial<State>);

const useCommonStore = <State>(
  makeInitStateFn: MakeInitStateFn<State>,
  initParams?: Partial<State>
) => {
  // 函式的方式彈性生成初始state
  const initState = makeInitStateFn(initParams);
  const state = ref(initState) as Ref<State>;

  // setState函式
  const setState = (newState: NewState<State>) => {
    const _newState =
      typeof newState === "function" ? newState(state.value) : newState;
    state.value = {
      ...state.value,
      ..._newState,
    };
  };

  const resetState = (partialRenewState?: Partial<State>) => {
    setState(makeInitStateFn(partialRenewState));
  };

  return {
    state,
    setState,
    resetState,
  };
};

export default useCommonStore;
```

```ts
// useCartStore

import useCommonStore, { MakeInitStateFn } from "./useCommonStore";

export interface SingleCommodity {
  name: string;
  price: number;
}

export interface CartStoreState {
  commodityWishList: SingleCommodity[];
}

const makeInitCartStoreState: MakeInitStateFn<CartStoreState> = (
  state
): CartStoreState => ({
  commodityWishList: [],
  ...state,
});

const useCartStore = definePiniaStore("cart", () => {
  // 直接引用就好!
  const { state, setState, resetState } = useCommonStore<CartStoreState>(
    makeInitCartStoreState
  );

  // add new item
  const addNewCommodity = (newCommodity: SingleCommodity) => {
    setState((s) => ({
      commodityWishList: [...s.commodityWishList, newCommodity],
    }));
  };

  // remove old item
  const removeCommodity = (commodityName: SingleCommodity["name"]) => {
    setState((s) => {
      const newWishList = [...s.commodityWishList];
      const idx = newWishList.findIndex(
        (commidity) => commidity.name === commodityName
      );
      if (idx === -1)
        return {
          commodityWishList: newWishList,
        };
      newWishList.splice(idx, 1);
      return {
        commodityWishList: newWishList,
      };
    });
  };

  const whishListTotalPrice = computed(() =>
    state.value.commodityWishList.reduce((p, n) => p + n.price, 0)
  );

  return {
    state,
    whishListTotalPrice,
    setState,
    resetState,
    addNewCommodity,
    removeCommodity,
  };
});

export default useCartStore;
```