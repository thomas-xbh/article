可以将store目录分为modules文件夹和index.ts;modules用来存放各个模块的数据,在index.ts中暴露出pinia的实例供各个模块使用

例:在各个模块中会暴露出一个无参函数,返回值是该模块实例,可以直接操作store中的数据

```vue
//index.ts
import { createPinia } from 'pinia';
const store = createPinia();
export { store };


//modules/user.ts
import { defineStore } from 'pinia';
import { store } from '/@/store';
export const useUserStore = defineStore({
  id: 'app-user',
  state: (): UserState => ({
    // user info
    userInfo: null,
  }),
  getters: {
    getUserInfo(state): UserInfo {
      return state.userInfo || getAuthCache<UserInfo>(USER_INFO_KEY) || {};
    },
  },
  actions: {
    setUserInfo(info: UserInfo | null) {
      this.userInfo = info;
      this.lastUpdateTime = new Date().getTime();
      setAuthCache(USER_INFO_KEY, info);
    },
  },
});

// Need to be used outside the setup
export function useUserStoreWithOut() {
  return useUserStore(store);
}

//index.vue
<script>
import { useUserStoreWithOut } from '/@/store/modules/user';
setup(){
    const userStore = useUserStoreWithOut();
}
</script>
```

