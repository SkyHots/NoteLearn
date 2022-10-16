### 1.vite


### 2.ref


### 3.reactive


### 4.setup
Vue3.2 setup ts 下结合mapState mapActions mapGetters mapMutations 使用
```
<script setup lang="ts">
import { computed } from "vue";
import { mapActions, mapGetters, mapMutations, mapState, useStore } from "vuex";

const store = useStore();
//computed属性结合mapState、mapGetters 
const [abc, list] = Object.values(mapState("user", [ "abc", "list"])).map(it=>computed(it.bind({$store:store}))) 
const [getList] = Object.values(mapGetters("user", [ "getList"])).map(it=>computed(it.bind({$store:store}))) 

//不做处理则直接返回bind对应的store方法 mapMutations mapActions 
const [incre] = Object.values(mapMutations("user", [ "incre"])).map(it=>(it.bind({$store:store}))) 
const [decre, getUser] = Object.values(mapActions("user", [ "decre","getUser"])).map(it=>it.bind({$store:store}))
<script>

<templete>
    <div>
        {{abc}}
    </div>
</templete>

```