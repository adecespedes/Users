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

<style lang="scss" src="../css/mixin.scss">
</style>
