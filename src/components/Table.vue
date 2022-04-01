<template>
  <div class="table-container">
    <table class="table">
      <thead>
      <tr>
        <th><input type="checkbox"></th>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Email</th>
        <th>Age</th>
        <th>Grade</th>
        <th>Actions</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in items" :key="item.id" >
        <td><input type="checkbox" v-model="checked" :id="item.id" :value="item.id" @change="checkBtn()"></td>
        <td>{{ item.firstName }}</td>
        <td>{{ item.lastName }}</td>
        <td>{{ item.email }}</td>
        <td>{{ item.age }}</td>
        <td>{{ item.grade }}</td>
        <td>
          <Edit style="color: #adad0e"/>
          <Delete style="color: red"/>
        </td>
      </tr>
      </tbody>
    </table>
    <div class="footer-tools">
      <div class="list-items">
        Showing {{ }} out of {{ }} entries
      </div>
      <div class="pages">
        <ul>
          <li><button>Previous</button></li>
          <li><button class="active">1</button></li>
          <li><button>2</button></li>
          <li><button>3</button></li>
          <li><button>4</button></li>
          <li><button>Next</button></li>
        </ul>
      </div>
    </div>
    {{ checked }}
  </div>

</template>

<script>
import Edit from 'vue-material-design-icons/Pencil'
import Delete from 'vue-material-design-icons/Delete'
export default {
  name: `Table`,
  components: {
    Edit,
    Delete
  },
  data () {
    return {
      items: [
        {
          id: '1',
          firstName: 'asasasas',
          lastName: 'ssssssss ssssssssssss',
          email: 'asadsdasd@gmail.com',
          age: '23',
          grade: '2nd'
        },{
        id: '2',
          firstName: 'asasasas',
          lastName: 'ssssssss ssssssssssss',
          email: 'asadsdasd@gmail.com',
          age: '23',
          grade: '2nd'
        },{
        id: '3',
          firstName: 'asasasas',
          lastName: 'ssssssss ssssssssssss',
          email: 'asadsdasd@gmail.com',
          age: '23',
          grade: '2nd'
        },{
        id: '4',
          firstName: 'asasasas',
          lastName: 'ssssssss ssssssssssss',
          email: 'asadsdasd@gmail.com',
          age: '23',
          grade: '2nd'
        },
      ],
      checked: []
    }
  },
  methods: {
    checkBtn(){
      this.$emit('disableBtn', this.checked)
    }
  }
}
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
.table-container .footer-tools .pages ul{
  padding: 0;
  margin: 0;
  display: flex;
  align-items: baseline;
  justify-content: flex-end;
}
.table-container .footer-tools .pages ul li{
  display: inline-block;
  margin: 0 2px;
}
.table-container .footer-tools .pages ul li button{
  width: 100%;
  box-sizing: border-box;
  border: 0;
  border-radius: 3px;
  background: transparent;
  cursor: pointer;
}
.table-container .footer-tools .pages ul li button:hover{
  background: #2c2cfd;
  color: white;
}
.table-container .footer-tools .pages ul li button.active{
  background-color: #2c2cfd;
  border-radius: 3px;
  color: white;
}
.table-container .footer-tools .pages ul li button,
.table-container .footer-tools .pages ul li span{
  padding: 6px 12px;
}


</style>