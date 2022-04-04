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
.table-container {
  border-collapse: collapse;
  width: 100%;
  color: #6f6f84;
  font-family: AnjaliOldLipi;
}
.table-container .footer-tools .pages button {
  border-collapse: collapse;
  width: 100%;
  color: #6f6f84;
  font-family: AnjaliOldLipi;
  font-size: 1rem;
}
.table-container .table {
  border-collapse: collapse;
  width: 100%;
  color: #6f6f84;
  font-family: AnjaliOldLipi;
}
.table-container .table,
.table-container .table th,
.table-container .table td {
  padding: 2px 2px;
}

.table-container .table th {
  font-weight: bolder;
  text-align: left;
  height: 50px;
  border-bottom: solid 2px #b6b6bb;
}
.table-container .table td {
  text-align: left;
  height: 50px;
  border-bottom: solid 1px #b6b6bb;
}

.table-container .table tbody tr:hover {
  background-color: #e1e1e7;
}

.btn-container ul {
  display: flex;
  justify-content: flex-start;
  margin: 0;
  align-items: baseline;
}
.btn-container ul li {
  display: inline-block;
  margin: 0 5px;
  align-items: baseline;
}
.btn:hover {
  cursor: pointer;
}

.table-container .footer-tools {
  padding: 12px;
  display: flex;
  justify-content: left;
  align-items: baseline;
}
.table-container .footer-tools .list-items {
  width: 50%;
}
.table-container .footer-tools .pages {
  margin-left: auto;
  margin-right: 0;
  width: 50%;
}
.table-container .footer-tools .pages ul {
  padding: 0;
  margin: 0;
  display: flex;
  align-items: baseline;
  justify-content: flex-end;
}
.table-container .footer-tools .pages ul li {
  display: inline-block;
  margin: 0 2px;
}
.table-container .footer-tools .pages ul li button {
  width: 100%;
  box-sizing: border-box;
  border: 0;
  border-radius: 3px;
  background: transparent;
  cursor: pointer;
}
.table-container .footer-tools .pages ul li button:hover {
  background: #2c2cfd;
  color: white;
}
.table-container .footer-tools .pages ul li button.active {
  background-color: #2c2cfd;
  border-radius: 3px;
  color: white;
}
.table-container .footer-tools .pages ul li button,
.table-container .footer-tools .pages ul li span {
  padding: 6px 12px;
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
.modal-default-button {
  padding: 5px;
}
</style>
