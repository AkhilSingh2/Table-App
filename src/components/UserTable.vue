<template>
    <div class="table__container">
        <table>
            <thead>
                <tr>
                    <th v-for="column in props.columns" :key="column.key" @click="$emit('sort',column.key)">
                        <div>
                            {{ column.text }}
                        </div>
                        <div><img class="sort-icon" width="48" height="48" src="https://img.icons8.com/color/48/sorting-arrows.png" alt="sorting-arrows"/></div>
                    </th>
                </tr>
            </thead>
            <tbody>
                <table-row v-for="(item, index) in props.data" :key="index">
                    <table-data v-for="column in props.columns" :key="column.key" :column="column.key">
                        <slot :name="column.key" :item="item">{{ item[column.key] }}</slot>
                    </table-data>
                </table-row>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import TableRow from './TableRow.vue';
import TableData from './TableData.vue';

const props = defineProps(['columns', 'data']);

</script>

<style scoped>
.sort-icon {
    width: 13px;
    height: 13px;
}
</style>