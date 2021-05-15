<template>
    <header>
        <h1>{{ title }}</h1>
        <Button text="Add Task" color="green" />
        <Button text="Add Task" color="blue" />
        <Button text="Add Task" color="red" />
    </header>
</template>
<script>
import Button from './Button'
export default {
    name: 'Header',
    props: {
        title: String,
    },
    components: {
        Button
    }
}
</script>
// scoped means it is only for this component
<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px; 
    }
</style>
