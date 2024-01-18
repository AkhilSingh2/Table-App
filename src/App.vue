<script setup>
import UserTable from '@/components/UserTable.vue'
import { onMounted, ref} from 'vue';
import axios from 'axios'

const columns = [
    {key: 'sno', text: 'S. No.'},
    {key: 'name', text: 'Name'},
    {key: 'picture', text: 'Picture'},
    {key: 'dob', text: 'DOB'},
    {key: 'email', text: 'Email'},
    {key: 'location', text: 'Location'},
    {key: 'phone', text: 'Phone'}
]
const users = ref([])
onMounted(async () => {
    axios.get('https://randomuser.me/api/?results=50').then((res) => {
        users.value = flattenJSON(res.data.results);
    })
})

const convertLocationObjectToString = (locationObj) => {
    const order = ['street.number', 'street.name', 'city', 'state', 'country', 'postcode'];
    return order.map(key => {
        const keys = key.split('.');
        let value = locationObj;
        for (const k of keys) {
            if (value && value[k] !== undefined) {
                value = value[k];
            } else {
                value = '';
                break;
            }
        }
        return value;
    }).join(', ');
}

const flattenJSON = (item) => {
    if(!item) return;
    return item.map((user, index) => ({
        sno: index + 1,
        name: `${user.name.first + ' '}${user.name.last}`,
        picture: user.picture.large,
        dob: user.dob.date,
        email: user.email,
        location: convertLocationObjectToString(user.location),
        phone: user.phone
    }))
}
const dateToString = (date) => {
    const tmp = new Date(date);
    return tmp.toLocaleDateString();
};
const sortOrder = ref('desc');

const sortColumn = (columnName) => {
  users.value = users.value.sort((a, b) => {
    const aValue = a[columnName];
    const bValue = b[columnName];

    if (sortOrder.value === 'asc') {
      return aValue > bValue ? 1 : -1;
    } else {
      return bValue > aValue ? 1 : -1;
    }
  });
  sortOrder.value = sortOrder.value === 'desc' ? 'asc' : 'desc';
};

</script>

<template>
  <div class="main">
    <UserTable :columns="columns" :data="users" @sort="sortColumn">
        <template #dob="slotProps">
            {{ dateToString(slotProps.item.dob) }}
        </template>
        <template #picture="slotProps">
            <img :src="slotProps.item.picture" :alt="slotProps.item.name"/>
        </template>
    </UserTable>
  </div>
</template>
<style scoped>
.main {
    width: 100%;
}
</style>