<template>
  <div class="table-container">
    <table class="table">
      <thead>
        <tr>
          <th><input type="checkbox" v-model="checkboxParent" @change="changeCheck()" /></th>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Email</th>
          <th>Age</th>
          <th>Grade</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.id">
          <td>
            <input
              type="checkbox"
              v-model="checked"
              :id="item.id"
              :value="item.id"
              @change="checkBtn()"
            />
          </td>
          <td>{{ item.first_name }}</td>
          <td>{{ item.last_name }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.age }}</td>
          <td>{{ item.grade }}</td>
          <td>
            <Edit class="btn" style="color: #adad0e" @click="editUser(item)" />
            <Delete
              class="btn"
              style="color: red"
              @click="deleteWindow(item.id)"
            />
          </td>
        </tr>
      </tbody>
    </table>
    <div class="footer-tools">
      <div class="list-items">Showing <b>{{ getPages }}</b> out of <b>{{ pagination.total }}</b> entries</div>
      <div class="pages">
        <ul>
          <li><button v-if="pagination.page !== 1" @click="loadData(pagination.page - 1)">Previous</button></li>
          <li v-for="i in pagination.last_page" :key="i">
            <button :class="pagination.page === i ? 'active' : ''" @click="loadData(i)">{{ i }}</button>
          </li>          
          <li><button v-if="pagination.page !== pagination.last_page" @click="loadData(pagination.page + 1)">Next</button></li>
        </ul>
      </div>
    </div>

    <ModalAdd :show="showDelete" styleContainer="width:400px;height:200px;">
      <template #header>
        <h3>Are you sure you want to delete the student?</h3>
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
              <button class="modal-default-button" @click="deleteUser">
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
import Edit from "vue-material-design-icons/Pencil";
import Delete from "vue-material-design-icons/Delete";
import axios from "axios";
import conf from "@/conf/conf";
import ModalAdd from "@/components/ModalAdd";
export default {
  name: `Table`,
  components: {
    Edit,
    Delete,
    ModalAdd,
  },
  data() {
    return {
      items: [],
      checked: [],
      showDelete: false,
      id: "",
      pagination: {
        total: '',
        page: 1,
        last_page: '',        
      },
      checkboxParent: false
    };
  },
  props: {
    refresh: {
      type: Boolean,
    },
  },
  watch: {
    refresh(newV) {            
      if (newV === true) {
        this.loadData();
      }
    },
  },
  mounted() {
    this.loadData();
  },
  computed: {
    getPages(){
      return this.pagination.last_page === this.pagination.page
       ? this.pagination.total
       : this.pagination.total >= 5 ? this.pagination.page * 5 : this.pagination.total
    }
  },
  methods: {
    changeCheck(){
      if(this.checkboxParent){
        this.items.map(x => {
          this.checked.push(x.id)
        })
        this.checkBtn()
      }
      else{
        this.checked = []
        this.checkBtn()
      }
    },
    deleteWindow(id) {
      this.showDelete = true;
      this.id = id;
    },
    async loadData(page) {
      let pages = 1
      if(page){
        pages = page
      }
      else{
        pages = this.pagination.page
      }

      try {
        const response = await axios.get(`${conf.api.services.users}?page=${pages}`);
        this.items = response.data.data;
        this.pagination.total = response.data.meta.total
        this.pagination.last_page = response.data.meta.last_page
        this.pagination.page = parseInt(response.data.meta.page)
      } catch (error) {}
    },
    editUser(data) {
      let user = {
        id: data.id,
        first_name: data.first_name,
        last_name: data.last_name,
        email: data.email,
        age: data.age,
        grade: data.grade,
      };
      this.$emit("edit", user);
    },
    async deleteUser() {
      try {
        const response = await axios.delete(`${conf.api.services.users}/${this.id}`);
        this.showDelete = false
        this.loadData();
      } catch (error) {}
    },
    checkBtn() {
      this.$emit("disableBtn", this.checked);
    },
  },
};
</script>

<style scoped>
@import '@/css/Table.scss'
</style>
