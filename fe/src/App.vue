<template>
  <el-container class="app">
    <el-header class="app-header">
      <p>Mock Server</p>
      <div class="links">
        <a href="https://github.com/funkyLover/mock-server" target="_blank"
          >GitHub</a
        >
        <a
          href="https://github.com/funkyLover/mock-server/blob/master/README.md"
          target="_blank"
          >Docs</a
        >
      </div>
    </el-header>
    <el-container>
      <el-aside class="app-menu" width="20%">
        <el-row>
          <el-col :span="24">
            <el-menu class="menu" :router="true" :default-active="$route.path">
              <el-menu-item index="/mocks">Mock data</el-menu-item>
              <el-menu-item index="/sets">Mock set</el-menu-item>
              <el-menu-item index="/proxys">Proxy config</el-menu-item>
            </el-menu>
          </el-col>
        </el-row>
      </el-aside>
      <el-main class="app-content">
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: 'app',
  components: {},
  mounted() {
    this.reqestMock();
  },
  methods: {
    async reqestMock() {
      const { $req, $awaitTo } = this;

      const req = $req({ url: '/$mock' });
      const { data } = await $awaitTo(req);

      this.$store.commit('updateMock', data);

      this.$requestId = setTimeout(() => {
        this.reqestMock();
      }, 1000);
    }
  }
};
</script>

<style lang="scss">
@import './styles/common/colors.scss';
@import './styles/app.scss';

.app .app-header {
  background: $black;
  color: #fff;
  font-weight: 600;
  font-size: 24px;
  align-items: center;
  display: flex;
  p {
    margin: 0;
  }

  .links {
    margin-left: 20px;
    flex: 1;
    text-align: right;

    a {
      color: #fff;
      font-size: 18px;
      font-weight: 400;
      text-decoration: none;
      margin-left: 20px;
    }

    a:hover {
      text-decoration: underline;
    }
  }
}

.app .app-menu {
  background: $white;
  max-width: 300px;
  min-width: 200px;
}
</style>
