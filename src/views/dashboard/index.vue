<template>
  <div class="dashboard-container">
    <div class="dashboard-text">账号: {{ name }}</div>
      <div>
        <table border="1">
          <tr>
            <th>编号</th>
            <th>编号</th>
            <th>编号</th>
          </tr>
        </table>
      </div>
  </div>
</template>

<script>
import user from '@/store/modules/user';
import { mapGetters,mapState } from "vuex";

export default {
  name: "Dashboard",
  data(){
    return{
      list:[]
    }
  },
  computed: {
    ...mapGetters(["name"]),
    ...mapState(["user"])
  },
};
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
