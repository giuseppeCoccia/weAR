<template>
<div>
  <el-header height="auto">
    <div class="headerRow1" v-if="!isCollapsed">
      <div class="top">
        <div class="col">
          <el-button size="small" v-if="!username" type="text" @click="loginDialogFormVisible = true">login</el-button>
        </div>
        <div class="col">
          <el-button size="small" v-if="!username" type="text" @click="signupDialogFormVisible = true">register</el-button>
        </div>
        <div class="col">
          <a v-if="username" :href="profile_page"><el-button size="small" type="text">{{ username }}</el-button></a>
        </div>
        <div class="col">
          <a v-if="username" :href="home_page" @click="logout"><el-button size="small" type="text">logout</el-button></a>
        </div>
      </div>
    </div>
    <div class="headerRow2" :class="{ paddedLogo: isCollapsed }">
      <a :href="home_page"><img :src="logo.asset" :alt="logo.alt" width="50px" height="50px"></a>
    </div>
    <div class="headerRow3">
      <div class="headerRow3_logo">
        <a :href="home_page"><img :src="logo.asset" :alt="logo.alt" width="25px" height="25px"></a>
      </div>
      <wear-nav/>
    </div>
  </el-header>
  <wear-login-form :visible.sync="loginDialogFormVisible"/>
  <wear-signup-form :visible.sync="signupDialogFormVisible"/>
</div>
</template>

<script>
import WearNav from './Nav'
import WearLoginForm from './LoginForm'
import WearSignupForm from './RegisterForm'

export default {
  name: 'wear-header',
  components: {
    'wear-nav': WearNav,
    'wear-login-form': WearLoginForm
  },
  data() {
    return {
      logo: getLogo(),
      signup_page: 'signup.html',
      home_page: 'index.html',
      login_page: 'login.html',
      profile_page: 'profile.html',
      isCollapsed: (window.innerWidth < 768),
      loginDialogFormVisible: false,
      signupDialogFormVisible: false
    }
  },
  computed: {
    username: function() {
      if (localStorage.loggedIn == 1) {
        return "Hi, " + localStorage.fullname
      }
      return null
    }
  },
  components: {
    'wear-nav': WearNav,
    'wear-login-form': WearLoginForm,
    'wear-signup-form': WearSignupForm
  },
  methods: {
    logout: function(e) {
      localStorage.loggedIn = 0
      return true
    },
    handleResize: function(event) {
      this.$data.isCollapsed = window.innerWidth < 768
    }
  },
  created() {
    window.addEventListener('resize', this.handleResize)
  },
  destroyed() {
    window.removeEventListener('resize', this.handleResize)
  },
}
</script>

<style>
.el-header {
  width: 100%;
  background: #fff;
}

.el-header .el-button {
  color: #303133;
  text-transform: uppercase;
}

/* Header row 1 */
.el-header .headerRow1, .el-header .headerRow2, .el-header .headerRow3 {
  width: 100%;
  display: inline-block;
  vertical-align: top;
  background: #fff;
}
.el-header .headerRow1 .col {
  margin-right: 20px;
  display: block;
  float: left;
}
.el-header .headerRow1 .top {
  float: right;
}

/* Header row 2 */
.el-header .headerRow2  {
  margin-top: -10px;
  margin-bottom: -5px;
  text-align: center;
}

.el-header .headerRow3 {
  width: 100%;
  height: 100%;
  margin-bottom: 5px;
  margin-top: 5px;
}

.el-header .headerRow3_logo{
  float: left;
  padding: 3px;
  display: none;
}

.paddedLogo {
  padding-top: 20px;
}
</style>
