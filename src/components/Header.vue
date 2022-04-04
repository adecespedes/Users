<template>
  <div>
    <header class="header">
      <p class="manage">Manage <b>Employees</b></p>
      <div class="buttons">
        <ul>
          <li>
            <button
              class="btnDel"
              :disabled="!checked.length > 0"
              @click="showWindowDelete"
            >
              <span class="ico icoDel">-</span><b>Delete</b>
            </button>
          </li>
          <li>
            <button class="btnAdd" @click="showWindow">
              <span class="ico icoAdd">+</span><b>Add New Employee</b>
            </button>
          </li>
        </ul>
      </div>
    </header>
    <Table @disableBtn="disableBtn" :refresh="refresh" @edit="editUser" />

    <ModalAdd :show="showModal" @close="showModal = false">
      <template #header>
        <h3>{{ getTitle }}</h3>
      </template>
      <template #body>
        <div class="input-container">
          <div class="row">
            <div class="column">
              <p>
                First Name:
                <input
                  type="text"
                  v-model="firstName"
                  placeholder="First Name"
                />
              </p>
              <p>
                Last Name:
                <input type="text" v-model="lastName" placeholder="Last Name" />
              </p>
              <p>
                Email:
                <input type="email" v-model="email" placeholder="Email" />
              </p>
            </div>
            <div class="column">
              <p>
                Age: <input type="number" v-model="age" placeholder="Age" />
              </p>
              <p>
                Grade: <input type="text" v-model="grade" placeholder="Grade" />
              </p>
            </div>
          </div>
        </div>
      </template>
      <template #footer>
        <div class="buttons">
          <ul>
            <li>
              <button class="modal-default-button" @click="closeWindow">
                CANCEL
              </button>
            </li>
            <li>
              <button class="modal-default-button" @click="addUser">OK</button>
            </li>
          </ul>
        </div>
      </template>
    </ModalAdd>

    <ModalAdd :show="showDelete" styleContainer="width:400px;height:200px;">
      <template #header>
        <h3>Are you sure you want to delete the selected student(s)?</h3>
      </template>
      <template #footer>
        <div class="buttons">
          <ul>
            <li>
              <button class="modal-default-button" @click="showDelete = false">
                CANCEL
              </button>
            </li>
            <li>
              <button class="modal-default-button" @click="deleteUsers">
                OK
              </button>
            </li>
          </ul>
        </div>
      </template>
    </ModalAdd>

    
  </div>
</template>

<script>
import Table from "@/components/Table";
import ModalAdd from "@/components/ModalAdd";
import axios from "axios";
import conf from "@/conf/conf";
export default {
  name: `Header`,
  components: { ModalAdd, Table },
  data() {
    return {
      showModal: false,
      disableDelete: false,
      checked: [],
      firstName: "",  
      lastName: "",
      email: "",
      age: "",
      grade: "",
      refresh: false,
      edit: false,
      id: "",
      title: '',
      showDelete: false
    };
  },
  computed: {
    getTitle(){
      return this.edit ? 'Edit User' : 'Add User'
    }
  },
  methods: {
    showWindowDelete(){
      this.showDelete = true
      this.refresh = false
    },
    closeWindow(){
      this.showModal = false
      this.firstName = ""
      this.lastName = ""
      this.email = ""
      this.age = ""
      this.grade = ""
    },
    showWindow() {
      this.showModal = true;
      this.edit = false; 
      this.refresh = false     
    },
    disableBtn(v) {      
      this.checked = v;
    },
    async deleteUsers() {
      let checked = this.checked      
       try {
         const response = await axios.delete(`${conf.api.services.users}/${this.checked}`);
         this.refresh = true;
         this.showDelete = false
      } catch (error) {}
    },
    async addUser() {
      /* eslint-disable */
      try {
        if (this.edit) {
          const response = await axios.put(
            `${conf.api.services.users}/${this.id}`,
            {
              first_name: this.firstName,
              last_name: this.lastName,
              email: this.email,
              age: this.age,
              grade: this.grade,
            }
          );
        } else {
          const response = await axios.post(conf.api.services.users, {
            first_name: this.firstName,
            last_name: this.lastName,
            email: this.email,
            age: this.age,
            grade: this.grade,
          });
        }
        
        this.refresh = true;
        this.closeWindow()
        
      } catch (error) {}
    },
    editUser(data) {
      this.showModal = true;
      this.firstName = data.first_name;
      this.lastName = data.last_name;
      this.email = data.email;
      this.age = data.age;
      this.grade = data.grade;
      this.id = data.id;
      this.edit = true;
      this.refresh = false
    },
  },
};
</script>

<style scoped>
.header {
  background-color: #0a256a;
  opacity: 80%;
  height: 70px;
  border-radius: 5px;
  padding: 10px;
}

.manage {
  color: white;
  float: left;
  font-size: 30px;
  margin-left: 20px;
  margin-top: 20px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.buttons ul {
  display: flex;
  justify-content: flex-end;
  margin: 0;
  padding: 15px;
  align-items: baseline;
}
.buttons ul li {
  display: inline-block;
  margin: 0 5px;
  align-items: baseline;
}
.buttons ul li button {
  cursor: pointer;
}

.btnDel:disabled,
.btnDel[disabled] {
  border: 1px solid #999999;
  background-color: #cccccc;
  color: #666666;
}

.btnAdd {
  height: 40px;
  width: 222px;
  color: white;
  background-color: green;
  display: inline-block;
  text-align: center;
  text-decoration: none;
  border: 1px solid transparent;
  font-size: 0.9rem;
  border-radius: 3px;
}
.btnDel {
  height: 40px;
  width: 117px;
  color: white;
  background-color: red;
  display: inline-block;
  text-align: center;
  text-decoration: none;
  border: 1px solid transparent;
  font-size: 0.9rem;
  border-radius: 3px;
}

.ico {
  position: absolute;
  width: 22px;
  height: 22px;
  font-size: 20px;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
  background: white;
  margin-left: -30px;
  margin-top: -2px;
}

.icoDel {
  color: red;
}
.icoAdd {
  color: green;
}

.input-container {
  display: flex;
  flex-direction: column;
  background-color: beige;
  border-radius: 3px;
  padding: 1em;
}
.input-container input {
  flex: 6;
  padding: 0.5em;
  margin-bottom: 1em;
}

.row {
  display: flex;
  justify-content: center;
}
.column {
  display: flex;
  flex-direction: column;
  padding: 1em;
}
.modal-default-button {
  padding: 5px;
}
</style>
