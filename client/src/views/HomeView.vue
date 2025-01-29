<script setup>
</script>

<template>
  <main>
    <button @click="showRegister()" v-if="user.username == ''" id="Register" class="register-btn">Register</button>
    <button @click="showLogin()" v-if="user.username == ''" id="Login" class="login-btn">Login</button>
    <button @click="logoutAcc()" v-if="user.username !== ''" id="Logout" class="logout-btn">Logout</button>
    <button @click="OpenPrivModal()" v-if="user.isAdmin" class = "set-priv-btn">Set priviledge</button>
    {{ user }}
    <div id="register_modal" class="register-modal">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="close-btn" @click="closeRegister()">
        <path fill="#eb0000"
          d="M64 32C28.7 32 0 60.7 0 96L0 416c0 35.3 28.7 64 64 64l384 0c35.3 0 64-28.7 64-64l0-320c0-35.3-28.7-64-64-64L64 32zM175 175c9.4-9.4 24.6-9.4 33.9 0l47 47 47-47c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9l-47 47 47 47c9.4 9.4 9.4 24.6 0 33.9s-24.6 9.4-33.9 0l-47-47-47 47c-9.4 9.4-24.6 9.4-33.9 0s-9.4-24.6 0-33.9l47-47-47-47c-9.4-9.4-9.4-24.6 0-33.9z" />
      </svg>
      <input class="username-input" v-model="register.username" type="text" placeholder='Username'>
      <div class="admin-selector-con">
        <label for="admin_selector" class="admin-txt">Set admin</label>
        <input type="checkbox" id="admin_selector" class="admin-selector" v-model="register.isAdmin" />
      </div>
      <button @click="registerAcc()" class = "create-btn">Create Account</button>
    </div>
    <!---->
    <div id="login_modal" class="login-modal">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="close-btn" @click="closeLogin()">
        <path fill="#eb0000"
          d="M64 32C28.7 32 0 60.7 0 96L0 416c0 35.3 28.7 64 64 64l384 0c35.3 0 64-28.7 64-64l0-320c0-35.3-28.7-64-64-64L64 32zM175 175c9.4-9.4 24.6-9.4 33.9 0l47 47 47-47c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9l-47 47 47 47c9.4 9.4 9.4 24.6 0 33.9s-24.6 9.4-33.9 0l-47-47-47 47c-9.4 9.4-24.6 9.4-33.9 0s-9.4-24.6 0-33.9l47-47-47-47c-9.4-9.4-9.4-24.6 0-33.9z" />
      </svg>
      <input class="username-input" v-model="login.username" type="text" placeholder='Username'>
      <button class = "create-btn" @click="loginAcc()">Login</button>
    </div>
    <!---->
    <div v-if="user.isAdmin" id="set_priv_modal" class="set-priv-modal">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="close-btn" @click="closePrivModal()">
        <path fill="#eb0000"
          d="M64 32C28.7 32 0 60.7 0 96L0 416c0 35.3 28.7 64 64 64l384 0c35.3 0 64-28.7 64-64l0-320c0-35.3-28.7-64-64-64L64 32zM175 175c9.4-9.4 24.6-9.4 33.9 0l47 47 47-47c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9l-47 47 47 47c9.4 9.4 9.4 24.6 0 33.9s-24.6 9.4-33.9 0l-47-47-47 47c-9.4 9.4-24.6 9.4-33.9 0s-9.4-24.6 0-33.9l47-47-47-47c-9.4-9.4-9.4-24.6 0-33.9z" />
      </svg>
      <input class="username-input" v-model="userPriviledge.username" type="text" placeholder='Username'>
      <button @click="searchUser()" class ="search-user-btn">Search User</button>
      <div v-if="foundUser" class="user-priv-info">
        <button @click="addEquipment()" class="priv-add-btn">Add Equipment</button>
        <div class="equipment-table">
          <div class="priv-add-tables" v-for="(item, index) in userPriviledge.userPriv.equipmentModel">
            <input class="priv-add-input" v-model="userPriviledge.userPriv.equipmentModel[index]" type="text"
              :key="index" :id="index" :placeholder="item">
            <button @click="deleteEquip(index)" class="item-del">Delete</button>
          </div>
        </div>
        <button @click="addDept()" class="priv-add-btn">Add Dept</button>
        <div class="priv-tables">
          <div class="priv-add-tables" v-for="(item, index) in userPriviledge.userPriv.deptName">
            <input class="priv-add-input" v-model="userPriviledge.userPriv.deptName[index]" type="text" :key="index"
              :id="index" :placeholder="item">
            <button @click="deleteDept(index)" class="item-del">Delete</button>
          </div>
        </div>
        <button @click="addgID()" class="priv-add-btn">Add gID</button>
        <div class="gID-table">
          <div class="priv-add-tables" v-for="(item, index) in userPriviledge.userPriv.gID">
            <input class="priv-add-input" v-model="userPriviledge.userPriv.gID[index]" type="text" :key="index"
              :id="index" :placeholder="item">
            <button @click="deletegID(index)" class="item-del">Delete</button>
          </div>
        </div>
        <button @click="savePriv()" class="set-btn">Give Priviledge</button>
      </div>
    </div>
    <!---->
    <div class="search-con" v-if="user.username !== ''">
      <input class="search-equip" type="search" placeholder="Search Equipment" v-model="searchEquip">
    </div>
    <div class="alc-table">
      <div v-for="alc in res" :data-target="alc['equipmentModel']" :key="alc['checkDate']" class="alc-row">
        {{ alc }}
      </div>
    </div>
  </main>
  <br />
  <br />
  <br />
  <div class="actual-data">
    <h1>Actual Data</h1>
    <div v-for="alc in res" :data-target="alc['equipmentModel']" :key="alc['checkDate']" class="actual-row">
      {{ alc }}
    </div>
  </div>
</template>

<script>
import { ref, watch } from 'vue'
import $ from 'jquery';
export default {
  data() {
    return {
      login: {
        username: '',
      },
      register: {
        username: '',
        isAdmin: false,
        userPriv: {
          equipmentModel: [],
          deptName: [],
          gID: []
        }
      },
      foundUser: false,
      searchEquip: ref(""),
      user: {
        username: '',
        isAdmin: false,
        userPriv: {
          equipmentModel: [],
          deptName: [],
          gID: []
        }
      },
      userPriviledge: {
        username: '',
        isAdmin: false,
        userPriv: {
          equipmentModel: [],
          deptName: [],
          gID: []
        }
      },
      res: ref([])
    }
  },
  methods: {
    OpenPrivModal() {
      $('body, html').css("background-color", "#747674bf")
      $('#set_priv_modal').css("display", "flex")
      $('#set_priv_modal').css("flex-direction", "column")
    },
    closePrivModal() {
      $('body, html').css("background-color", "")
      $('#set_priv_modal').hide()
      this.foundUser = false
    },
    searchUser() {
      let existingUser = localStorage.getItem(this.userPriviledge.username)
      console.log(existingUser)
      if (existingUser !== null) {
        let userObj = JSON.parse(existingUser)
        this.userPriviledge.userPriv = userObj.userPriv
        this.foundUser = true
      }
      else {
        alert("user not found")
        this.foundUser = false
      }
    },
    savePriv() {
      localStorage.setItem(this.userPriviledge.username, JSON.stringify(this.userPriviledge))
      alert("priviledge Set Successfully");
      this.searchUser();
    },
    showRegister() {
      $('body, html').css("background-color", "#747674bf")
      $('#register_modal').css("display", "flex")
      $('#register_modal').css("flex-direction", "column")
    },
    closeRegister() {
      $('body, html').css("background-color", "")
      $('#register_modal').hide();
    },
    registerAcc() {
      console.log(this.register)
      if (localStorage.getItem(this.register.username) == null) {
        localStorage.setItem(this.register.username, JSON.stringify(this.register));
        alert("account created successfully");
        this.closeRegister();
      } else {
        alert("account already existed");
      }
    },
    showLogin() {
      $('body, html').css("background-color", "#747674bf")
      $('#login_modal').css("display", "flex")
      $('#login_modal').css("flex-direction", "column")
    },
    closeLogin() {
      $('body, html').css("background-color", "")
      $('#login_modal').hide()
    },
    logoutAcc() {
      this.user.username = ''
      this.user.isAdmin = false
      this.user.userPriv.equipmentModel = []
      this.user.userPriv.deptName = []
      this.user.userPriv.gID = []
      this.foundUser = false
      this.restrictViews()
    },
    loginAcc() {
      let curUser = localStorage.getItem(this.login.username);
      if (curUser !== null) {
        this.user = JSON.parse(curUser);
        this.closeLogin();
        this.restrictViews()
      } else {
        alert("login failed");
      }
    },
    addEquipment() {
      this.userPriviledge.userPriv.equipmentModel.push('')
    },
    deleteEquip(index) {
      this.userPriviledge.userPriv.equipmentModel.splice(index, 1)
    },
    addDept() {
      this.userPriviledge.userPriv.deptName.push('')
    },
    deleteDept(index) {
      this.userPriviledge.userPriv.deptName.splice(index, 1)
    },
    addgID() {
      this.userPriviledge.userPriv.gID.push('')
    },
    deletegID(index) {
      this.userPriviledge.userPriv.gID.splice(index, 1)
    },
    restrictViews() {
      let userPriv = this.user.userPriv
      let privSet = new Set(userPriv.equipmentModel)
      if (this.user.isAdmin == false) {
        $(document).ready(function () {
          if (userPriv.equipmentModel.length == 0) {
            $(".alc-table").hide();
          } else {
            $(".alc-table").show();
            $(".alc-table").children().each(function () {
              let equipmentNo = $(this).attr('data-target');
              console.log(equipmentNo);
              if (privSet.has(equipmentNo)) {
                console.log("showing " + equipmentNo)
                $(this).show()
              } else {
                //console.log("no")
                $(this).hide()
              }
            })
          }
        })
      } else {
        $(document).ready(function () {
          $(".alc-table").show();
          $(".alc-table").children().each(function () {
            $(this).show()
          })
        })
      }
    },
    filterByEquipments(searchLower) {
      if (this.user.isAdmin) {
        $(".alc-table").children().each(function () {
          let equipmentNo = $(this).attr('data-target');
          if (equipmentNo.includes(searchLower)) {
            $(this).show()
          } else {
            $(this).hide()
          }
        })
      } else {
        let privSet = new Set(this.user.userPriv.equipmentModel)
        $(".alc-table").children().each(function () {
          let equipmentNo = $(this).attr('data-target');
          if (equipmentNo.includes(searchLower) && privSet.has(equipmentNo)) {
            $(this).show()
          } else {
            $(this).hide()
          }
        })
      }
    },
    getLocalDb() {
      let existData = localStorage.getItem("alcoholRes")
      if (existData !== null) {
        let parseData = JSON.parse(existData)
        this.res = parseData;
      }
    },
    async getAlchohol(url) {
      return fetch(url, {
        method: "POST"
      })
        .then(response => {
          if (response.status == 200) {
            return response.json()
          }
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          throw error;
        });
    }
  },
  mounted() {
    this.getLocalDb();
    this.restrictViews()
    setInterval(() => {
      this.getAlchohol("http://127.0.0.1:5013/actionapi/User").then(data => {
        if (this.res.length > 0) {
          let lastPerson = this.res[this.res.length - 1]
          if (lastPerson['checkDate'] !== data['checkDate']) {
            this.res.push(data)
            localStorage.setItem("alcoholRes", JSON.stringify(this.res))
          }
        } else {
          this.res.push(data)
          localStorage.setItem("alcoholRes", JSON.stringify(this.res))
        }
        if (this.searchEquip == '') {
          console.log("enabling")
          this.restrictViews()
        }
        //console.log(data)
      })
    }, 2000)

    watch(() => this.searchEquip, () => {
      //let searchLower = this.searchEquip.toLowerCase();
      this.filterByEquipments(this.searchEquip)
    })
  }
}
</script>

<style scoped>
.register-btn {
  width: 8vw;
  height: 2vw;
  font-size: 1vw;
  color: green;
  margin: 2vw 1vw 1vw 0vw;
  background-color: white;
  border-radius: 10px;
  border: 1px solid green;
  cursor: pointer;
}

.login-btn {
  width: 8vw;
  height: 2vw;
  font-size: 1vw;
  color: white;
  margin: 2vw 1vw 1vw 0vw;
  background-color: green;
  border-radius: 10px;
  border: 1px solid green;
  cursor: pointer;
}

.logout-btn {
  width: 8vw;
  height: 2vw;
  font-size: 1vw;
  color: white;
  margin: 2vw 1vw 1vw 0vw;
  background-color: darkgreen;
  border-radius: 10px;
  border: 1px solid darkgreen;
  cursor: pointer;
}

.set-priv-btn{
  width: 8vw;
  height: 2vw;
  font-size: 1vw;
  color: white;
  margin: 2vw 1vw 1vw 0vw;
  background-color: red;
  border-radius: 10px;
  border: none;
  cursor: pointer;
}

.close-btn {
  width: 2vw;
  position: fixed;
  /*top: -0.6vw;
    left: 19.7vw;*/
  top: -2.2%;
  left: 90.6%;
  z-index: 1000;
  cursor: pointer;
}

.register-modal {
  display: none;
  /* Initially hidden */
  position: fixed;
  justify-content: space-around;
  top: 40%;
  width: 20vw;
  height: 10vw;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  z-index: 1000;
}
.username-input{
  margin: 1vw 0vw 0vw 0vw;
  padding: 0.3vw;
  font-size: 0.8vw;
  border-radius: 1px;
  border: 1px solid green;
}

.admin-selector-con{
  display: flex;
}
.admin-selector{
  margin-left: 1vw;
  width: 1vw;
}

.create-btn{
  width: 7vw;
  height: 2vw;
  position: fixed;
  /*top: -0.6vw;
    left: 19.7vw;*/
  top: 70%;
  left: 60%;
  cursor: pointer;
  background-color: rgb(11, 167, 107);
  border-radius: 5px;
  font-size: 0.9vw;
  color: white;
  border: none;
}
.login-modal {
  display: none;
  /* Initially hidden */
  position: fixed;
  justify-content: space-between;
  top: 40%;
  width: 20vw;
  height: 8vw;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  z-index: 1000;
}

.set-priv-modal {
  display: none;
  /* Initially hidden */
  position: fixed;
  align-items: center;
  justify-content: space-between;
  top: 50%;
  width: 20vw;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  z-index: 1000;
}

.search-user-btn{
  width: 7vw;
  height: 2vw;
  margin: 1vw;
  /*top: -0.6vw;
    left: 19.7vw;*/
  cursor: pointer;
  background-color: rgb(11, 167, 107);
  border-radius: 5px;
  font-size: 0.9vw;
  color: white;
  border: none; 
}

.priv-add-btn{
  width: 6vw;
  height: 1.7vw;
  margin: 1vw 0vw 0.2vw 0vw;
  /*top: -0.6vw;
    left: 19.7vw;*/
  cursor: pointer;
  background-color: rgb(58, 132, 218);
  border-radius: 5px;
  font-size: 0.7vw;
  color: white;
  border: none; 
}

.priv-add-input{
  margin: 0.5vw 0vw 0vw 0vw;
  padding: 0.3vw;
  font-size: 0.8vw;
  border-radius: 1px;
  border: 1px solid rgb(58, 132, 218);
}

.item-del{
  margin: 0.5vw 0vw 0vw 0vw;
  cursor: pointer;
  background-color: rgb(240, 48, 0);
  font-size: 0.8vw;
  padding: 0.2vw;
  font-size: 0.8vw;
  border-radius: 4px;
  color: white;
  border: none;
}

.priv-add-tables{
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.set-btn{
  width: 13vw;
  height: 2vw;
  margin: 2vw 0vw 1vw 0vw;
  /*top: -0.6vw;
    left: 19.7vw;*/
  cursor: pointer;
  background-color: rgb(58, 132, 218);
  border-radius: 5px;
  font-size: 0.9vw;
  color: white;
  border: none; 
}

.admin-selector-con {
  display: flex;
}

.search-equip{
  padding: 0.3vw;
  font-size: 0.8vw;
  margin: 1vw 0vw 1vw 0vw;
}
.alc-table {
  display: flex;
  flex-direction: column;
}

.alc-row {
  width: 100%;
}

.actual-data {
  display: flex;
  flex-direction: column;
}

.actual-row {
  width: 100%;
}
</style>