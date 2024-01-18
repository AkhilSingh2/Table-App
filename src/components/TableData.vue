<template>
    <td>
        <slot></slot>
    </td>
</template>
<script setup>
const props = defineProps(['column'])
</script>