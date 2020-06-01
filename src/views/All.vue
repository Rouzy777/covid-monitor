<template>
    <div>
        <router-link to="/">Back</router-link>
        <Table class='mt-4'/>
    </div>
</template>

<script>
import Table from '@/components/Table.vue'

export default {
    name: 'Home',
    components: {
        Table
    }
}
</script>
