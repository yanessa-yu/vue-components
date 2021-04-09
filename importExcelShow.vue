<template>
    <table>
        <tr v-for="(value,index) in list">
            <td v-for="(v, i) in value">{{ v }}</td>
        </tr>
    </table>
    
</template>

<script>
export default {
    props:['list']
    
}
</script>

<style scoped>
table{margin: 0 .2rem;}
td{
    border: 1px solid #ddd;
    padding: .2rem;
}

</style>


